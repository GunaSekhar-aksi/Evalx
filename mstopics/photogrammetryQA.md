# Interview Q&A: Photogrammetry platform (Metashape SDK wrapper)

Each answer has four parts:
- **Our app:** what is really built.
- **Gap/bug:** a weakness I know about. Say it before they find it.
- **Robust fix:** what I would build.
- **Why:** the reasoning.

Items marked **(verify)** are things I did not confirm in code. Check them before you claim them.

---

## Part 1. Core architecture (the 12 questions)

### 1. Why split Orchestra and the worker? What breaks if Redis dies mid-job?
- **Our app:** Orchestra is a FastAPI service with no SDK. The worker is a synchronous Metashape SDK process that needs a licence and a GPU. Jobs run for hours, so the API must never block on them. The two scale independently.
- **If Redis dies:**
  - A running job keeps going, because the worker writes status straight to Postgres.
  - Progress pub/sub is lost, so the UI falls back to polling `/jobs/{id}/status`.
  - Tickets still waiting in Redis are lost if Redis has no persistence.
  - The result list (`metashape_results`) can lose a message.
- **Gap:** Redis is a second source of truth for "queued".
- **Fix:**
  - Postgres is the source of truth and Redis is only a delivery channel.
  - Turn on Redis AOF (`appendonly yes`, `everysec`).
  - The reconciler re-enqueues any job that is `queued` in the DB but missing from Redis for longer than N minutes **(verify exactly what our reconciler does)**.
  - Make the worker's final DB write idempotent.
- **Why:** you can rebuild Redis from the DB, but not the other way round.

### 2. How do you stop a job from being lost or run twice?
- **Our app:**
  - Producers `rpush` to a lane list.
  - The worker moves the ticket into a per-slot `metashape_processing:slot-N` list (reliable-queue pattern). A crash leaves the ticket there for recovery.
  - Heartbeat monitor: a job is stuck after 5 minutes without a heartbeat, and a queued ticket is requeued after 30 minutes.
  - One active job per plan, checked at the API (403 `JOB_ACTIVE`).
- **Gap:** the "one active job" check and the insert are not atomic. Two fast clicks, or two API callers, could both pass. The worker also has no fencing, so a worker that was only slow, not dead, could run alongside its replacement.
- **Fix:**
  - A partial unique index in Postgres: `UNIQUE (plan_id) WHERE status IN ('queued','running')`.
  - A `lease_token` on the job. The worker's writes must carry it (`UPDATE ... WHERE id=? AND lease_token=?`), so a zombie worker's writes are rejected.
- **Why:** the database enforces the rule even if the app code has a race. Fencing is the standard cure for "at-least-once delivery plus a slow worker".

### 3. Why no ORM? Why psycopg3 async in the API and psycopg2 in the worker?
- **Our app:**
  - Raw SQL through repositories gives full control of queries, no hidden N+1, and easy bulk updates for job steps.
  - The API is async, so it needs the psycopg3 async pool.
  - The worker is a synchronous, long-running process driven by the SDK. It has no event loop, so psycopg2 is the simple fit.
- **Gap:** two drivers means duplicated SQL and schema knowledge, and `keys.py` is a byte-identical copy in both repos.
- **Fix:** extract a small shared package (storage keys, job-status enums, SQL for job updates) and version it. Add a CI check that fails if the two copies drift.
- **Why:** the drift risk is silent, since a key mismatch gives "file not found" at runtime.

### 4. How do you scale beyond one licence? What limits throughput?
- **Our app:**
  - One licence per worker slot.
  - Three lanes: `light` (quick edits), `project` (derived rasters, no licence needed), `metashape` (opens the .psx, needs the licence).
  - More worker machines mean more slots and more licences.
- **Limits:** licence seats, GPU memory, shared-storage I/O, and the 6 h job timeout.
- **Gap:** I have no real benchmarks. The calculator constants are placeholders.
- **Fix:**
  - Record real per-stage durations (photos, megapixels, quality, GPU) from the jobs table. Use them for queue ETAs and pricing.
  - Autoscale GPU workers on queue depth, with each worker holding a seat.
  - Keep non-licence work on cheaper lanes.
- **Why:** licence cost dominates (`pricing-plan.md`), so keeping Metashape busy and moving everything else off it is the main economic lever.

### 5. How is progress streamed? What if the WebSocket drops?
- **Our app:** the worker publishes `job_progress:{id}` on Redis pub/sub. The Orchestra `pubsub_listener` forwards it to `/ws/jobs/{id}`. The frontend polls `/jobs/{id}/status` as a fallback.
- **Gap:** pub/sub is fire-and-forget, so a client that reconnects misses events.
- **Fix:** on connect, the server first sends a snapshot from the DB (status, step, percent), then streams live events. The client reconnects with exponential backoff.
- **Why:** the DB is the durable state and the stream is only an optimisation. The UI can never get stuck on stale progress.

### 6. Alignment and dense-cloud parameters and trade-offs
- **Alignment:**
  - Accuracy low/medium/high maps to downscale 4/2/1 (higher accuracy means more time).
  - Keypoint limit 1k–100k, capped at 40k above 1,500 photos.
  - Tiepoint limit 500–20k.
  - Preselection speeds up matching on big sets.
  - The 40k cap and the preselection rule exist to protect memory and time.
- **Dense cloud:**
  - Quality low/medium/high is depth-map downscale 8/4/2. Each step up costs roughly 4x points, time and memory.
  - Filtering modes trade detail against noise.
  - A GPU depth-map failure triggers a CPU fallback.
- **Validation:** three layers (frontend, Pydantic strict, worker sanitiser that never raises).
- **Why:** out-of-range advanced values can OOM a 5-hour job, so they are rejected early and clamped late.

### 7. How is volume computed? Accuracy and pitfalls?
- **Our app:** `POST /outputs/{id}/volume` runs rasterio on the DEM or DTM. It masks the polygon, picks a base elevation (perimeter mean by default), then sums `(z - base) × pixel_area`. Cut is terrain above base and fill is terrain below. Net is cut minus fill. Pixel size and the CRS come from the raster.
- **Known bugs:**
  1. `base_method="custom"` is accepted but ignored, so it silently becomes `min`.
  2. A second, unused implementation (`_compute_volume_sync`) uses a different pixel-area formula.
  3. The frontend type expects fields the backend does not return.
  4. A frontend comment says net = fill − cut, but the backend does cut − fill.
- **Pitfalls:**
  - A perimeter base is wrong on sloped ground (a best-fit plane is better).
  - Nodata (-10000) cells inside the polygon bias the result.
  - Geographic CRS pixels are not equal area.
  - Vertical error is multiplied by the area.
- **Fix:**
  - Keep one implementation and delete the other.
  - Honour `custom`, or reject it with a 422.
  - Add a best-fit-plane base.
  - Return `nodata_fraction` and warn above a threshold.
  - Reproject to a metric CRS first.
  - Test against a synthetic DEM with a known volume (a cone, a box).
  - Echo `base_method` in the response.
- **Why:** volume is the number customers bill from. A silent wrong answer is worse than an error.

### 8. How would you add outbound webhooks, service tokens, server-side AOI?
- **Our app:** none exist. Notifications go in-app, by email, SMS and WhatsApp. Auth is user-based. The site boundary lives in the browser.
- **Webhooks:**
  - A `webhook_endpoints` table (url, secret, events).
  - An outbox table written in the same transaction as the job status change.
  - A delivery worker with HMAC-SHA256 signature, exponential retry, a dead-letter state and an event id for dedupe.
  - Why an outbox: no lost events and no phantom events.
- **Service tokens:**
  - Use the IDM app-registration model as the base.
  - Add scoped API keys (hashed in the DB) owned by an org or service account, with a per-key rate limit and quota.
  - Why: one human login per machine caller is not auditable or revocable.
- **AOI:**
  - Add a `geometry` column on plan (PostGIS, or validated GeoJSON jsonb).
  - Validate with `ST_IsValid`, a size cap and a vertex cap.
  - Volume and compare then read the stored AOI. They no longer depend on a browser.

### 9. Security
- **Our app:**
  - Orchestra checks email and password. IDM validates every token, with a per-app IP allow-list (max 5).
  - Report downloads use HMAC-signed URLs (1 hour, `uid:exp:sig`).
  - Tiles, `<img>` and WebSockets take the user token in `?token=`.
- **Gaps:**
  - Tokens in query strings leak into access logs and referrers.
  - The signing key is the general `secret_key`.
  - A per-user upload limit exists, but there is no tenant isolation beyond user ownership checks.
- **Fix:**
  - Short-lived, single-output-scoped tile tokens (5–15 min).
  - A separate signing key, with `hmac.compare_digest` for checks **(verify what we use)**.
  - Strip `token` from access logs.
  - Add an automated test that every output route checks ownership.
- **Why:** a leaked tile URL would otherwise expose a full session.

### 10. How do you test the worker without a Metashape licence?
- **Our app:**
  - The SDK import is guarded (`try: import Metashape except ImportError: Metashape = None`).
  - Pure pieces run without it and are testable: volume, NDVI, D8 drainage, overlap math, DEM fill and the sanitisers.
  - `BaseClassifier` and `BaseMaskGenerator` are interfaces, so fakes slot in.
- **Gap:** SDK calls (keyword names for `buildDem`, `exportRaster` and others) were not run against Metashape 2.3.x. Breakline and the manual reports are untested on a real licence.
- **Fix:**
  - A thin `MetashapeAdapter` that fakes the SDK in unit tests.
  - A contract test suite that runs on the licensed machine nightly.
  - Small golden datasets (about 20 photos) with expected tolerances.
  - A smoke job in CI on the GPU box.
- **Why:** mocks prove our logic, and only the golden run proves the SDK usage.

### 11. Retention, storage cost, S3
- **Our app:**
  - Outputs expire after 30 days (`outputs.expires_at`, `expiry_worker` hourly).
  - Local or NFS storage works today, and S3 is behind an abstraction (`app/storage`). The roadmap marks S3 as not yet implemented.
  - I did not find a rule that expires photos or the .psx.
- **Gap:** photos and projects can grow without limit, and they are large.
- **Fix:**
  - S3 lifecycle rules (Standard to Infrequent Access to Glacier).
  - Keep raw photos and the .psx per a plan-level retention setting.
  - Quota from "bytes used" so it is storage-agnostic.
  - A pre-expiry notification with an export option.
  - The worker needs a local scratch workspace, since the SDK wants a POSIX path. The workspace setting exists but is off by default.
- **Why:** storage cost is quiet and cumulative, and quota tied to tiers makes it a billing feature.

### 12. A 2,000-photo job fails at hour 5
- **Our app:**
  - The worker timeout is 6 hours.
  - The heartbeat monitor detects a stuck job.
  - Retry downgrades quality (downscale up to 16).
  - The job and its steps are marked failed, and a notification and a PDF report are available.
- **Gap:** a retry restarts from scratch, so 5 hours are lost, and the downgrade silently lowers quality.
- **Fix:**
  - Stage checkpoints. Save the .psx after alignment and after the dense cloud, and record `last_completed_step`. A retry resumes from the last good stage.
  - Show the user the real failure reason and say clearly if quality was downgraded.
  - Do not bill failed minutes.
  - Alert on repeated GPU failures.
- **Why:** most failures happen late, in the heaviest stage. Resuming turns a 5-hour loss into minutes.

---

## Part 2. More questions

### 13. Measurements and site boundary live in localStorage. Problem?
- **Our app:** `meas_{planId}_{outputId}` and `boundary_{planId}` are browser-only.
- **Bug:**
  - Clearing the browser loses everything.
  - Teammates cannot see each other's work.
  - Two devices disagree.
  - Reports and automation cannot read them.
- **Fix:**
  - A server table `measurements(plan_id, output_id, type, geometry, props jsonb, created_by, version)` with CRUD endpoints.
  - Migrate localStorage once on first load.
  - Use optimistic concurrency (`version`) for edits.
- **Why:** it is a prerequisite for collaboration, for the automation link, and for server-side reports.

### 14. Compare is computed in the browser. Is that OK?
- **Our app:** two calls to the volume endpoint over the same polygon, then a client subtraction. A 9×9 grid samples elevation differences.
- **Bug:**
  - There is no check that the two runs share a CRS or alignment.
  - The result is not stored.
  - The sign convention differs between the frontend comment and the backend.
- **Fix:** a server `POST /plans/{id}/compare` that verifies CRS, extent overlap and resolution. It resamples to a common grid, computes a DEM-of-difference with an uncertainty threshold, and stores the result.
- **Why:** change detection must be reproducible and auditable.

### 15. Two Metashape machines, two writers to Postgres. Consistency risk?
- **Our app:** the worker writes the job state directly. Orchestra also writes (cancel, retry, expiry).
- **Risk:** lost updates, for example a cancel racing with a "completed" write.
- **Fix:**
  - A job state machine with allowed transitions enforced in SQL (`UPDATE ... WHERE status IN (...)`, check the affected rows).
  - Terminal states are final.
  - Use `updated_at` or a version column.
- **Why:** it makes illegal transitions impossible regardless of who writes.

### 16. GCP correction. How do you keep it safe?
- **Our app:**
  - Needs at least 3 control points, each with 3 or more confirmed marks, and aligned photos.
  - The worker rejects a result that drops any camera or more than 10% of tie points.
  - Control and check points are separate.
  - ArUco auto-detect exists, though the GCP doc calls it out of scope.
- **Gap:** no stated accuracy numbers.
- **Fix:** store the check-point RMSE per run and show it in the report. Gate "survey-grade" claims on that number.
- **Why:** accuracy is the product's promise, so it needs a measured, stored figure.

### 17. DTM on stockpiles or heaps
- **Our app:** `ClassifyGroundPoints` with max angle 15°, max distance 1 m and cell size 50 m, then `buildDem` using ground only.
- **Risk:** a heap can be classified as ground or not, depending on size against cell size and the angle. This is untested.
- **Fix:**
  - Test on a real pile.
  - Offer a "stockpile mode" preset (smaller cell, tuned angle).
  - Let the user paint ground and non-ground seeds.
  - Compare against a base from a pre-pile survey.
- **Why:** volume accuracy depends on the base surface, not the arithmetic.

### 18. Upload reliability on bad networks
- **Our app:**
  - A 4-step session with a 2-hour TTL.
  - Per-file confirm, a content-hash dedupe (first and last 1 MiB plus size), and an upload cancel route.
  - The client caps (500 MB, 5,000 files, 50 GB) are enforced only in the browser.
- **Gap:** the server does not re-check extension or size, and a hash on 2 MiB can in theory collide on edited files.
- **Fix:**
  - Server-side format and size checks.
  - Optional full-file checksum at confirm.
  - Resumable chunked upload (as video already has).
  - Return a clear `session_expired` that offers a resume.
- **Why:** the browser is untrusted, and API clients will not have your client-side checks.

### 19. Rate limiting and abuse
- **Our app:** 20 requests per minute per IP on unauthenticated routes, and 600 per minute per user on upload routes.
- **Gap:** no per-account job or compute quota, and credits are simulated, so nothing gates processing.
- **Fix:**
  - Enforce a per-tier concurrent-job limit and a monthly GPU-minute quota in the job-creation transaction.
  - Reserve credits at submit and settle them at finish.
  - Refund failed jobs.
- **Why:** a single user can currently occupy the only licence.

### 20. Observability
- **Our app:** the `audit` router, heartbeats, step records and logs.
- **Gap:** no metrics or tracing were visible to me **(verify)**.
- **Fix:**
  - Request and job IDs in every log line, passed through the Redis ticket so one trace spans API and worker.
  - Metrics: queue depth per lane, job duration per stage, failure rate, GPU fallback count, licence wait time.
  - Alerts for a stuck lane and for a rising downgrade rate.
- **Why:** without queue-depth and stage-duration metrics, scaling is guesswork.

### 21. How would you price and prove unit economics?
- **Our app:** `pricing-plan.md` has real infrastructure and licence numbers (about $3 per processing hour on a savings plan). The in-app calculator uses placeholder rates.
- **Fix:** replace the placeholders with measured GPU-minutes per stage, then add margin. Settle credits from measured time, not estimates.
- **Why:** pricing from guesses can lose money on large jobs.

### 22. Huge outputs in the browser
- **Our app:** XYZ PNG tiles (EPSG:3857, 256 px), and a Potree viewer for point clouds, with PLY and GLB viewers.
- **Risks:** large tile counts, memory pressure in 3D, and a token on every tile request.
- **Fix:**
  - Cloud-Optimised GeoTIFF with range requests.
  - Pre-built tile pyramids and CDN caching, with signed short-lived URLs.
  - Level-of-detail for clouds (Potree octree).
- **Why:** it cuts load, bandwidth and server CPU together.

### 23. Coordinate systems and heights
- **Our app:** the chunk CRS comes from EXIF (WGS84) or from GCPs. Tiles are 3857 and contours are exported in 4326. I found no geoid or vertical-datum handling.
- **Risk:** drone EXIF altitude is ellipsoidal, and other systems use orthometric heights. Mixing them gives metre-level errors in elevation and in cut/fill against a surveyed design.
- **Fix:**
  - Store the horizontal and vertical CRS per output.
  - Apply a geoid model when a vertical datum is requested.
  - Show the datum in reports and downloads.
- **Why:** it is invisible to users until it breaks a survey.

### 24. Why Redis lists and not Kafka?
- **Our app:** a decision recorded in the roadmap: stay on Redis.
- **Answer:** the load is dozens of heavy jobs, not millions of events. Kafka brings operational weight without benefit. Lists give priority lanes, a simple reliable queue, and easy inspection. I would revisit it only for event fan-out (webhooks, analytics).

### 25. An automated caller (the satellite app) wants to use it. What changes?
- **Gap list:** service auth, a stored AOI, webhooks, a server-side chain, quota policy.
- **Fix (order):**
  1. Scoped API keys.
  2. AOI on the plan.
  3. A `POST /pipelines` endpoint that takes an AOI and an upload session. It enqueues align, then cloud, then DEM and ortho, then volume, then the report, as one tracked pipeline. The browser batch hook does this today.
  4. Outbound webhooks.
  5. A server-rendered report.
  6. Credit enforcement.
- **Why:** each step removes a browser dependency, so the whole flow runs unattended.

### 26. Mocked or spec-only features I must not claim
- 19 frontend functions have `USE_MOCK` branches (off in `.env`).
- `PM.md` (the 80% GPS threshold, auto-tuning, blur rejection) is spec.
- Credits are simulated.
- The Razorpay keys are test keys.
- The forgot-password endpoint is a stub.
- The marketing KPIs ("99.3%", "1.6 cm") are hardcoded.
- No LiDAR, canopy or biomass logic exists.
- No design-surface cut/fill, density, tonnes or material detection exists.

### 27. The migration incident (0055/0056)
- **Our app:** two migrations were added and then deleted in history. The chain broke and was repaired (head 0058).
- **Lesson:** parallel branches will claim the same number.
- **Fix:** CI that checks for a single head, with a migration lint. Reserve numbers, or use timestamped names. Never delete an applied migration; add a corrective one.

### 28. Idempotency of API calls
- **Gap:** `POST /process` and `POST /align` have no idempotency key, so a retry after a network timeout could create a second job.
- **Fix:** accept an `Idempotency-Key` header, store `(key, user, request hash) to job_id` for 24 hours, and return the original response on repeat.
- **Why:** it is required for unattended callers and for flaky mobile networks.

### 29. What would you do differently?
1. Put measurements and the AOI on the server from day one.
2. Make Postgres the single owner of job state, with lease tokens.
3. Take benchmarks early, so pricing and ETAs are real.
4. Share one package for keys and enums.
5. Add stage checkpoints for resumable jobs.

### 30. A customer says "my volume is wrong". How do you debug?
1. Check the output type (DEM or DTM) and run id.
2. Check the base method. `custom` was ignored in the live path.
3. Check the nodata fraction inside the polygon.
4. Check the CRS and pixel size.
5. Check GCP check-point RMSE and the DTM ground classification around the pile.
6. Re-run the calculation on a synthetic shape of known volume.
7. Compare against a manual TIN calculation in QGIS.

---

## 60-second summary for the interview
"It is a SaaS around the Metashape SDK. A React frontend talks to a FastAPI orchestrator. The orchestrator queues jobs in Redis lanes. A licensed GPU worker runs the SDK. Postgres holds the job state and Redis is only the delivery channel. Progress streams over WebSocket, with polling as the fallback. It outputs orthomosaics, DEM and DTM, point clouds, meshes, NDVI, contours, volume, reports and comparisons. The weak spots I know about are browser-only measurements, no service auth or webhooks, simulated credits, an unfinished volume base option, and no benchmarks. I would fix them in this order: job state ownership, stage checkpoints, server-side AOI and measurements, API keys, then webhooks."
