# MSSD — System Design: LLD + HLD (Microsoft)

> Weightage: ~5–10% for freshers (often LLD/OOP-flavored, or a design question that collapses into DSA),
> much higher for SDE-1 with experience and SDE-2. Reported: "design a system where a client in space
> sends images to a server in India", replication internals, connection pools, traffic spikes, autoscaling.

## The framework (use it for every design question)
1. **Clarify requirements:** functional (what it does) + non-functional (scale, latency, consistency, availability). Ask the numbers: users, requests per second, data size.
2. **Rough estimates:** QPS, storage per day. Only to the order of magnitude.
3. **API:** the 3–5 main endpoints.
4. **Data model + storage choice** (see MSDBMS.md #5).
5. **High-level diagram:** client -> LB/gateway -> services -> cache -> DB, plus a queue if anything is async.
6. **Deep dive** wherever they push: scaling, failures, bottlenecks.
7. **Trade-offs:** say what you gave up. A design with no trade-offs is a design you didn't think through.

## 1. "Client in space sends images to a server in India" (reported, SDE1)
They're testing whether you notice the constraints, not whether you draw boxes:
- **Huge latency, intermittent link** (ground-station passes are only minutes long). So: **chunk** images, give each chunk an ID and checksum, and make uploads **resumable**. On reconnect, send only the missing chunks.
- **Limited bandwidth:** compress on board, prioritize (thumbnails first, full resolution later), queue everything locally.
- **Reliability:** use acknowledgments per chunk, idempotent writes (resending a chunk is harmless), and reassemble on the ground.
- **Ground side:** ingest service -> object storage (blobs) + a metadata DB (image id, timestamp, status) -> a queue for processing.

## 2. Handling sudden traffic spikes (SDE-1 report)
- **Absorb:** a queue in front of slow work (accept the request, process it async), and cache hot reads (Redis/CDN).
- **Scale:** autoscale stateless services horizontally. Keep services stateless so any instance can serve any request.
- **Protect:** rate limit at the gateway, use circuit breakers on calls to dependencies, and degrade gracefully (show cached or partial data instead of failing).
- **Autoscaling triggers (asked):** CPU or memory crossing a threshold (e.g. 70%), request rate, queue length, or a custom metric like p99 latency. Mention a **cooldown** so the system doesn't thrash between scaling up and down.

## 3. Connection pooling + singleton repository (SDE-1 report)
Opening a DB connection is expensive (TCP + auth). A **pool** keeps N connections open and lends them out. It's literally a counting semaphore (see MSOS). The **repository** that owns the pool is a singleton, so the whole app shares one pool instead of each class creating its own and exhausting the DB's connection limit.
Size it to what the DB can handle. More connections past that point just add contention.

## 4. Caching
- **Cache-aside (most common):** read the cache; on a miss, read the DB and populate the cache.
- **Write-through:** write to the cache and DB together. **Write-back:** write to the cache, flush to the DB later (fast, but risks data loss).
- **Eviction:** LRU (see MSDSA #1), LFU, TTL.
- **The hard part:** invalidation. Stale data, plus a **thundering herd** when a hot key expires and 1000 requests hit the DB at once. Fixes: lock or single-flight on the miss, staggered TTLs.

## 5. CAP theorem
When there's a network **P**artition, you must choose **C**onsistency (reject requests) or **A**vailability (serve possibly stale data). P isn't optional in a distributed system.
Example: a bank balance chooses C. A social-media like count chooses A.

## 6. Design a URL shortener (classic warm-up)
- **API:** POST /shorten -> code; GET /{code} -> 301/302 redirect.
- **Code generation:** base62 of an auto-increment ID or a range-allocated counter (no collisions), or a hash plus a collision check.
- **Storage:** key-value (code -> URL). Very read-heavy, so cache aggressively.
- **301 vs 302:** a 301 gets cached by browsers (less load, but you lose click analytics). A 302 hits you every time.

## 7. Design a rate limiter
- **Token bucket:** tokens refill at rate r up to a cap of b; each request spends a token. Allows bursts.
- **Sliding window log/counter:** more accurate, costs more memory.
- **Distributed:** keep counters in Redis with atomic increments. Return a 429 with Retry-After.

## 8. Message queues
They decouple producer from consumer, absorb spikes, and allow retries. Know these terms:
- **At-least-once delivery** means consumers must be **idempotent** (processing the same message twice is harmless).
- A **dead-letter queue** holds messages that keep failing.
- Ordering is guaranteed only per partition.

## 9. LLD questions (freshers get these)
Parking lot, elevator, library management, Snake & Ladder, vending machine. They're checking:
- Clean class breakdown with single responsibility.
- Interfaces wherever behavior varies (e.g. a PricingStrategy).
- Correct relationships (composition vs inheritance; see MSOOP).
- One or two design patterns used where they actually fit: Strategy, Factory, Observer, Singleton, State. Don't force a pattern in just to show you know it.

## 10. "What do you understand by distributed systems?" (reported)
Multiple machines working together and appearing as one system. You gain scale and fault tolerance. You pay with: networks fail, clocks disagree, partial failures happen, and consistency gets hard.
Name-drop carefully, and only what you can explain: replication, sharding, consensus (Raft), idempotency.
