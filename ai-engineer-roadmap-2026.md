# The AI Engineer Bible — 2026

A complete, opinionated roadmap. Read top to bottom once. Then use it as a checklist.

---

## Table of Contents

0. [Read This First](#0-read-this-first)
1. [What an AI Engineer Actually Is](#1-what-an-ai-engineer-actually-is)
2. [The Skill Map (Overview)](#2-the-skill-map-overview)
3. [Layer 0 — Programming Foundations](#3-layer-0--programming-foundations)
4. [Layer 1 — Math You Actually Need](#4-layer-1--math-you-actually-need)
5. [Layer 2 — Data Engineering Basics](#5-layer-2--data-engineering-basics)
6. [Layer 3 — Classical Machine Learning](#6-layer-3--classical-machine-learning)
7. [Layer 4 — Deep Learning](#7-layer-4--deep-learning)
8. [Layer 5 — Transformers & How LLMs Work](#8-layer-5--transformers--how-llms-work)
9. [Layer 6 — Using LLMs (The Actual Job)](#9-layer-6--using-llms-the-actual-job)
10. [Layer 7 — RAG (Retrieval-Augmented Generation)](#10-layer-7--rag-retrieval-augmented-generation)
11. [Layer 8 — Agents & Tool Use](#11-layer-8--agents--tool-use)
12. [Layer 9 — Evaluation (The Skill Nobody Has)](#12-layer-9--evaluation-the-skill-nobody-has)
13. [Layer 10 — Fine-Tuning & Model Adaptation](#13-layer-10--fine-tuning--model-adaptation)
14. [Layer 11 — Inference, Serving & Performance](#14-layer-11--inference-serving--performance)
15. [Layer 12 — LLMOps, Observability & Cost](#15-layer-12--llmops-observability--cost)
16. [Layer 13 — AI System Design](#16-layer-13--ai-system-design)
17. [Layer 14 — Security & Safety](#17-layer-14--security--safety)
18. [Layer 15 — Multimodal & Beyond Text](#18-layer-15--multimodal--beyond-text)
19. [Layer 16 — Optional Depth: Training Models](#19-layer-16--optional-depth-training-models)
20. [The Tool Stack — What to Learn, What to Skip](#20-the-tool-stack--what-to-learn-what-to-skip)
21. [Project Ladder](#21-project-ladder)
22. [Month-by-Month Study Plan](#22-month-by-month-study-plan)
23. [Portfolio, Resume & Public Presence](#23-portfolio-resume--public-presence)
24. [Interview Preparation](#24-interview-preparation)
25. [Learning Resources](#25-learning-resources)
26. [Anti-Patterns — How People Waste a Year](#26-anti-patterns--how-people-waste-a-year)
27. [Glossary](#27-glossary)
28. [Self-Assessment Checklist](#28-self-assessment-checklist)

---

## 0. Read This First

**Three rules that govern this entire document.**

**Rule 1 — Depth beats breadth.** You cannot learn every item here to expert level. You need *working literacy* in most of it and *deep mastery* in about four areas: LLM application engineering, RAG, evaluation, and production deployment. Everything else exists so you understand what you're doing and why.

**Rule 2 — Build, don't watch.** Courses create the feeling of progress without the substance. The rule: for every hour of tutorial, spend three hours building something the tutorial didn't cover. If you cannot break it and fix it, you don't know it.

**Rule 3 — Ship to production.** A notebook that works on your laptop proves nothing. A deployed service with monitoring, error handling, and cost controls proves everything. The gap between "prototype" and "production" is where careers are made and lost in this field.

**How to use this doc:** Do not read it as a linear syllabus and try to complete Layer 0 to 100% before touching Layer 1. That's how you spend eight months on math and never build anything. Instead:

- Skim everything once so you know the shape of the field.
- Get to "functional" on Layers 0–2 (weeks, not months).
- Then jump to Layers 6–9. That's the job.
- Backfill Layers 3–5 as you hit things you don't understand.
- Layers 10–14 as you get serious.

---

## 1. What an AI Engineer Actually Is

### The definition

An AI engineer **builds systems that use foundation models as components**.

Compare:
- **Software engineer** — builds software.
- **Data scientist** — derives insights from data.
- **ML engineer** — trains and deploys custom models on custom data.
- **Research scientist** — invents new model architectures and training methods.
- **AI engineer** — ships AI-powered features into products.

The AI engineer role is **closer to backend engineering than to research**. If you enjoy shipping, debugging, and system design, this is your lane. If you enjoy proving theorems and reading arXiv all day, you want research — different path, usually requires a PhD.

### What the day actually looks like

- Debugging why a retrieval pipeline is returning irrelevant chunks.
- Writing an eval harness that grades 500 model outputs automatically.
- A/B testing two system prompts.
- Cutting inference cost by 60% by routing easy queries to a smaller model.
- Building a tool-calling loop that doesn't get stuck in a cycle.
- Explaining to a PM why the model hallucinated and what it costs to fix.
- Setting up caching, retries, timeouts, and fallbacks around a flaky API.

Notice: **very little of that is model training.** Almost none of it is math.

### The role variants you'll see in job posts

| Title | What it really means |
|---|---|
| AI Engineer | Ships LLM features. This doc. |
| ML Engineer | Trains and deploys models, more classical ML + MLOps |
| Applied AI Engineer | Customer-facing; helps others build on your company's model |
| Research Engineer | Builds infra for training frontier models; heavy systems + ML |
| Research Scientist | Invents new methods; usually PhD |
| LLM/GenAI Engineer | Same as AI Engineer, different branding |
| Forward Deployed Engineer | AI engineer who works on-site with customers |
| AI Product Engineer | AI engineer with more product ownership |

### Honest market context

- The title barely existed as a job category before 2023. It's now one of the fastest-growing roles.
- Compensation is above software engineering median, meaningfully so at frontier labs.
- **Entry-level pure "AI engineer" roles are rare.** Most postings want 3–5+ years of software engineering, with LLM experience layered on top. The realistic path for a junior: get hired as a backend/fullstack engineer, become the person on the team who ships AI features, and title-change into it within 1–2 years.
- Beware hype-driven hiring. Many "AI Engineer" postings are ordinary backend jobs with a `openai.chat.completions.create()` call in them.

---

## 2. The Skill Map (Overview)

```
┌──────────────────────────────────────────────────────────────┐
│  L14 Safety   L15 Multimodal   L16 Training (optional depth)  │
├──────────────────────────────────────────────────────────────┤
│  L11 Inference   L12 LLMOps   L13 System Design               │  ← Senior
├──────────────────────────────────────────────────────────────┤
│  L9 Evaluation      L10 Fine-tuning                           │  ← Differentiators
├──────────────────────────────────────────────────────────────┤
│  L6 LLM APIs   L7 RAG   L8 Agents                             │  ← THE JOB
├──────────────────────────────────────────────────────────────┤
│  L3 Classical ML   L4 Deep Learning   L5 Transformers         │  ← Understanding
├──────────────────────────────────────────────────────────────┤
│  L0 Programming   L1 Math   L2 Data                           │  ← Foundation
└──────────────────────────────────────────────────────────────┘
```

**Priority weighting for a job search:**

| Layer | Weight | Why |
|---|---|---|
| L6 LLM APIs | ★★★★★ | This is the daily work |
| L7 RAG | ★★★★★ | Most common production pattern |
| L9 Evaluation | ★★★★★ | Rarest skill, biggest differentiator |
| L0 Programming | ★★★★★ | Non-negotiable baseline |
| L8 Agents | ★★★★☆ | Fastest-growing area |
| L12 LLMOps | ★★★★☆ | Separates prototypes from products |
| L13 System Design | ★★★★☆ | Interview-critical |
| L11 Inference | ★★★☆☆ | Matters at scale |
| L5 Transformers | ★★★☆☆ | Conceptual understanding, not implementation |
| L2 Data | ★★★☆☆ | Quietly critical |
| L10 Fine-tuning | ★★★☆☆ | Often the wrong answer, but you must know when it's right |
| L14 Safety | ★★★☆☆ | Rising fast |
| L3 Classical ML | ★★☆☆☆ | Literacy level |
| L4 Deep Learning | ★★☆☆☆ | Literacy level |
| L1 Math | ★★☆☆☆ | Enough to read, not to derive |
| L15 Multimodal | ★★☆☆☆ | Nice-to-have, growing |
| L16 Training | ★☆☆☆☆ | Only for research track |

---

## 3. Layer 0 — Programming Foundations

**Goal: You are a competent software engineer first. AI second.**

This is the layer people skip because it's "boring." It's also the layer that gets people rejected. A company will hire a strong engineer who learned LLMs in three months over a prompt-engineering enthusiast who can't write a clean service.

### 3.1 Python (primary language)

Non-negotiable competency:

- **Core language** — data types, comprehensions, generators, decorators, context managers, `*args/**kwargs`, closures, exceptions.
- **Typing** — type hints, `typing` module, `Optional`, `Union`, generics. Use `mypy` or `pyright`. Untyped Python in production is a liability.
- **Async** — `asyncio`, `async/await`, `asyncio.gather`, semaphores for concurrency limits. **Critical**, because LLM calls are I/O-bound. Sequential API calls when you could parallelize is the single most common junior performance mistake.
- **Data modeling** — Pydantic (v2). Used everywhere in the AI stack for structured outputs and config validation.
- **Packaging & environments** — `uv` (fast, now the default choice), `poetry`, or `pip` + `venv`. Understand `pyproject.toml`, lock files, dependency resolution.
- **Testing** — `pytest`, fixtures, parametrize, mocking, `pytest-asyncio`. Test LLM code by mocking the API layer.
- **Standard library fluency** — `json`, `re`, `pathlib`, `itertools`, `functools`, `dataclasses`, `collections`, `logging`.
- **Performance** — profiling with `cProfile`, understanding when to reach for `numpy` vectorization instead of loops.

### 3.2 A second language (pick one)

- **TypeScript** — if you'll touch frontends or build with the JS AI SDKs. Highest practical value.
- **Go** — high-throughput services, great concurrency, common in infra teams.
- **Rust** — inference engines, tokenizers, performance-critical paths. Steep curve, high signal.

Don't learn all three. One, properly.

### 3.3 Software engineering practice

- **Git** — branching, rebase vs merge, resolving conflicts, bisect, writing atomic commits, PR hygiene. If `git rebase -i` scares you, fix that this week.
- **Linux/CLI** — bash, pipes, `grep`/`sed`/`awk`, process management, `ssh`, `systemd` basics, file permissions, environment variables.
- **APIs** — REST design, HTTP semantics, status codes, idempotency, pagination, auth (API keys, OAuth2, JWT), rate limiting, webhooks. Server-Sent Events (SSE) specifically — that's how LLM streaming works.
- **Databases** — SQL (joins, indexes, query plans, transactions, isolation levels), Postgres specifically. Know when NoSQL/Redis is appropriate.
- **Frameworks** — FastAPI (the default for Python AI services). Understand dependency injection, background tasks, middleware, streaming responses.
- **Concurrency & queues** — task queues (Celery, RQ, or cloud-native), message brokers (Redis, RabbitMQ, SQS), worker pools. Long-running LLM jobs belong in queues, not request handlers.
- **Design patterns & clean code** — SOLID at a practical level, dependency inversion, repository pattern, avoiding god-objects. Not dogma, just don't write spaghetti.
- **Debugging** — `pdb`, structured logging, reading stack traces, binary search debugging, reproducing before fixing.

### 3.4 Infrastructure basics

- **Docker** — writing Dockerfiles, multi-stage builds, layer caching, `docker compose`, volumes, networking, image size optimization.
- **Cloud (pick one: AWS, GCP, or Azure)** — compute (EC2/Compute Engine), object storage (S3/GCS), managed Postgres, secrets management, IAM, and at least one serverless option (Lambda/Cloud Run). Cloud Run and similar are excellent for AI services.
- **CI/CD** — GitHub Actions. Automated tests, linting, build, deploy on merge.
- **Kubernetes** — *later*. Learn it when you need it. Most AI services run fine on Cloud Run / ECS / a single VM. Premature k8s is a time sink.
- **Infrastructure as code** — Terraform basics. Nice-to-have, not urgent.

### 3.5 Checkpoint — you're done with L0 when

- [ ] You can build and deploy a typed, tested, containerized FastAPI service with a Postgres backend, in a day, without a tutorial.
- [ ] You can explain the difference between concurrency and parallelism and pick correctly.
- [ ] Your git history looks like an adult wrote it.
- [ ] You can debug a production error from logs alone.

---

## 4. Layer 1 — Math You Actually Need

**Goal: Read papers and docs without panic. Not: derive backpropagation from scratch.**

Be honest about the target. You need to understand *what things mean*, not prove theorems. If a job requires deriving gradients by hand, that's research, not AI engineering.

### 4.1 Linear algebra (highest ROI)

- Vectors, vector spaces, dot product, norms (L1, L2), cosine similarity.
- Matrices, matrix multiplication, transpose, inverse, identity.
- **Matrix shapes and broadcasting** — the single most practically useful skill. 90% of your tensor bugs are shape errors.
- Eigenvalues/eigenvectors — conceptual only.
- Dimensionality reduction: PCA, SVD — conceptual, plus t-SNE/UMAP for visualizing embeddings.

**Why it matters:** embeddings are vectors, attention is matrix math, similarity search is cosine distance. This is the language of the field.

### 4.2 Probability & statistics

- Random variables, distributions (normal, Bernoulli, uniform, categorical).
- Conditional probability, Bayes' theorem.
- Expectation, variance, standard deviation.
- Sampling, sampling bias.
- Hypothesis testing, p-values, confidence intervals.
- **Statistical significance in A/B tests** — you will run these on prompts.
- Softmax and what "temperature" does to a distribution — directly relevant to LLM sampling.
- Entropy, cross-entropy, KL divergence — cross-entropy is *the* loss function of language modeling.

### 4.3 Calculus

- Derivatives, partial derivatives, chain rule.
- Gradients, gradient descent intuition.
- Local vs global minima, saddle points.
- That's it. You will never compute a derivative by hand at work. You need the *intuition* for why learning rates matter.

### 4.4 Optimization

- Loss functions, objective functions.
- Gradient descent variants: SGD, momentum, Adam, AdamW.
- Learning rate schedules, warmup, cosine decay.
- Regularization: L1/L2, dropout, early stopping.
- Overfitting vs underfitting, bias-variance tradeoff.

### 4.5 Information theory (light)

- Entropy, perplexity (the classic LLM metric — lower is better).
- Cross-entropy loss.
- KL divergence — appears in RLHF/DPO objectives.

### 4.6 Discrete math / CS theory

- Big-O notation, complexity analysis.
- Graphs (agent workflows are graphs).
- Trees (tokenizer tries, search trees).
- Hashing (deduplication, caching, LSH for approximate nearest neighbors).

### 4.7 How much time to spend

**Two to four weeks, part-time.** Not six months. Watch 3Blue1Brown for linear algebra and neural networks, work through enough problems to make it stick, then move on. Come back when something specific confuses you. Math is a *reference*, not a *gate*.

---

## 5. Layer 2 — Data Engineering Basics

**Goal: Get data in, clean it, keep it clean. Underrated. Quietly decides project outcomes.**

### 5.1 Data manipulation

- **pandas** — loading, filtering, groupby, merge, pivot, apply, missing values. Know its performance limits.
- **polars** — faster modern alternative. Increasingly the default for large data.
- **numpy** — arrays, broadcasting, vectorized ops, `argsort`, `argmax`.
- **DuckDB** — SQL directly over Parquet/CSV files. Extremely useful and underused.

### 5.2 Formats & storage

- JSON, JSONL (the standard for datasets and eval sets), CSV, Parquet (columnar, compressed, use it), Arrow.
- Object storage patterns (S3/GCS): prefixes, lifecycle rules, presigned URLs.
- When to use: file → SQLite → Postgres → warehouse.

### 5.3 Text processing (critical for LLM work)

- Encoding hell: UTF-8, normalization (NFC/NFKC), mojibake, BOM.
- Cleaning: whitespace, HTML stripping, boilerplate removal.
- Document parsing: **this is genuinely hard**. PDFs (pypdf, pdfplumber, PyMuPDF, Unstructured, and the newer VLM-based parsers), DOCX, HTML, Markdown. PDF extraction quality is often the actual bottleneck in a RAG system, not the model.
- OCR for scanned documents (Tesseract, cloud OCR APIs, or a vision model).
- Chunking strategies (covered in depth in L7).
- Deduplication: exact hashing, MinHash/LSH for near-duplicates.
- PII detection and redaction — you will need this.

### 5.4 Pipelines

- ETL vs ELT concepts.
- Batch vs streaming.
- Orchestration: Airflow, Dagster, or Prefect. Learn one, lightly.
- Idempotency, retries, checkpointing, backfills.
- Data validation: Pydantic for schemas, Great Expectations if you get serious.

### 5.5 Data quality principles

- Garbage in, garbage out — applies more to LLM systems than anything else.
- Provenance and lineage: where did this document come from, when, what version.
- Freshness: stale data is wrong data.
- Licensing and rights: can you legally use this corpus? Real legal exposure.
- Privacy: GDPR, DPDP Act (India), data residency requirements.

---

## 6. Layer 3 — Classical Machine Learning

**Goal: Literacy. You need to recognize when a problem doesn't need an LLM.**

This layer is deprioritized *not because it's useless* but because it's rarely your daily work. Still, an AI engineer who reaches for a 200B-parameter model to classify emails into three categories — when logistic regression would do it faster, cheaper, and more accurately — is an expensive liability.

### 6.1 Core concepts

- Supervised vs unsupervised vs reinforcement learning.
- Features, labels, training/validation/test splits.
- Cross-validation, k-fold.
- Overfitting, underfitting, regularization.
- Data leakage — the classic silent killer.
- Class imbalance and how to handle it.

### 6.2 Algorithms (understand the intuition + when to use)

**Supervised:**
- Linear regression, logistic regression
- Decision trees
- Random forests
- **Gradient boosting (XGBoost, LightGBM, CatBoost)** — still beats deep learning on tabular data. Know this.
- k-Nearest Neighbors
- Support Vector Machines
- Naive Bayes

**Unsupervised:**
- k-Means, hierarchical clustering, DBSCAN
- PCA, UMAP, t-SNE
- Anomaly detection (Isolation Forest)

### 6.3 Evaluation metrics

- **Classification:** accuracy, precision, recall, F1, ROC-AUC, PR-AUC, confusion matrix. Know when accuracy lies (imbalanced classes).
- **Regression:** MAE, MSE, RMSE, R².
- **Ranking/retrieval:** Precision@k, Recall@k, MRR, NDCG. **These matter for RAG.** Learn them properly.
- Calibration — are your confidence scores meaningful?

### 6.4 Tooling

- **scikit-learn** — the whole classical stack. Pipelines, transformers, `GridSearchCV`.
- Feature engineering: encoding categoricals, scaling, binning, interaction terms.
- Experiment tracking: MLflow or Weights & Biases.

### 6.5 The judgment call

Ask before every LLM: *could a classifier, a regex, a database query, or a rules engine do this?* Often yes. Deterministic beats probabilistic when deterministic works.

---

## 7. Layer 4 — Deep Learning

**Goal: Understand neural networks well enough to read code and debug training. You'll rarely train from scratch.**

### 7.1 Fundamentals

- Neurons, layers, weights, biases.
- Activation functions: ReLU, GELU, SiLU/Swish, sigmoid, tanh, softmax. Know why ReLU replaced sigmoid.
- Forward pass, loss computation, backpropagation, weight update.
- Batch size, epochs, iterations, steps.
- Initialization (Xavier/He) — why it matters.
- Normalization: BatchNorm, LayerNorm, RMSNorm (used in modern LLMs).
- Residual/skip connections — why very deep networks are trainable at all.
- Dropout.
- Vanishing/exploding gradients, gradient clipping.

### 7.2 Architectures (know what each is for)

- **MLP / feedforward** — the baseline.
- **CNN** — images, spatial data. Convolution, pooling, receptive field.
- **RNN / LSTM / GRU** — sequences. Largely historical now, but explains *why* transformers won.
- **Transformer** — everything. Own layer below.
- **Autoencoders / VAEs** — compression, representation learning.
- **GANs** — generation. Mostly superseded by diffusion.
- **Diffusion models** — image/video/audio generation. Know the concept: iterative denoising.
- **Graph Neural Networks** — niche but real.

### 7.3 Frameworks

- **PyTorch** — the default. Learn: tensors, `autograd`, `nn.Module`, `DataLoader`/`Dataset`, optimizers, training loop, `.to(device)`, `no_grad()`, saving/loading checkpoints.
- **JAX** — used at research labs (functional, `jit`, `vmap`, `grad`). Learn only if you go research-track.
- TensorFlow/Keras — legacy in most new work. Skip unless a job requires it.

### 7.4 Practical training skills

- GPU basics: CUDA, VRAM, what OOM means and how to fix it (smaller batch, gradient accumulation, mixed precision, gradient checkpointing).
- Mixed precision (fp16/bf16), quantization concepts (int8, int4).
- Hyperparameter tuning: learning rate first, always.
- Reading loss curves: is it learning, overfitting, diverging, or stuck?
- Transfer learning and freezing layers.
- Reproducibility: seeds, determinism, environment pinning.

### 7.5 How deep to go

Build one small model end-to-end in PyTorch (MNIST classifier, then a small text classifier). Understand the training loop cold. Then stop — unless you're going the research route, you don't need to implement ResNet from scratch.

---

## 8. Layer 5 — Transformers & How LLMs Work

**Goal: Understand the machine you'll spend your career operating. Conceptual mastery, one implementation for depth.**

You can be a productive AI engineer without this. You cannot be a *good* one. When your system behaves strangely, this layer is what lets you reason about why instead of guessing.

### 8.1 Tokenization

- What a token is. Why "strawberry" has a famously awkward relationship with counting r's.
- BPE (Byte-Pair Encoding), WordPiece, SentencePiece, tiktoken.
- Vocabulary size tradeoffs.
- Why tokenization affects: cost, context limits, non-English performance (English is cheaper per word than most languages), arithmetic, and character-level tasks.
- Special tokens: BOS, EOS, padding, chat templates.
- **Practical:** count tokens before you send them. Budget context like memory.

### 8.2 Embeddings

- Dense vector representations of meaning.
- Word embeddings (Word2Vec, GloVe) → contextual embeddings (BERT) → modern text embedding models.
- Embedding dimensions, and the tradeoff between dimension and cost.
- Cosine similarity vs dot product vs Euclidean distance.
- Matryoshka embeddings (truncatable dimensions).
- Embedding models are *not* the same as generative models. Different tools.

### 8.3 The transformer architecture

Learn these components and what each does:

- **Self-attention** — Query, Key, Value. The core idea: every token looks at every other token and decides what's relevant.
- **Scaled dot-product attention** — and why the scaling factor exists.
- **Multi-head attention** — parallel attention subspaces.
- **Positional encoding** — sinusoidal → learned → **RoPE (Rotary)** and ALiBi. RoPE is what modern models use and it's why context extension is possible.
- **Feed-forward network** — the per-token MLP; where much of the "knowledge" lives.
- **Layer norm / RMSNorm**, pre-norm vs post-norm.
- **Residual stream** — the conceptual backbone; important for interpretability.
- **Causal masking** — why decoders can't see the future.

Variants:
- **Encoder-only** (BERT) — classification, embeddings.
- **Decoder-only** (GPT, Claude, Llama) — generation. This is what "LLM" means today.
- **Encoder-decoder** (T5) — translation, seq2seq.

Efficiency variants worth knowing by name:
- Multi-Query Attention (MQA), Grouped-Query Attention (GQA) — cheaper inference.
- FlashAttention — memory-efficient exact attention.
- Sliding window / sparse attention.
- **Mixture of Experts (MoE)** — sparse activation; how models get large parameter counts with manageable inference cost.
- State space models (Mamba) — alternative architecture, watch this space.

### 8.4 Training stages (know the pipeline)

1. **Pretraining** — next-token prediction on enormous text corpora. This is where capability comes from. Costs millions of dollars.
2. **Supervised Fine-Tuning (SFT)** — instruction following on curated demonstrations.
3. **Preference optimization** — RLHF (reward model + PPO), DPO, and successors. This is where "helpful, harmless, honest" behavior gets shaped.
4. **Reasoning training / RL on verifiable rewards** — the newer stage that produces extended-thinking models.

Understand the concepts. You will not do stages 1 or 4 at a normal company.

### 8.5 Inference mechanics

- **Autoregressive generation** — one token at a time, each conditioned on all previous.
- **KV cache** — why the first token is slow and the rest are fast; why long contexts eat memory.
- **Prefill vs decode** — two distinct phases with different bottlenecks (compute-bound vs memory-bandwidth-bound).
- **Sampling parameters:**
  - `temperature` — flattens or sharpens the distribution. 0 = deterministic-ish, high = chaotic.
  - `top_p` (nucleus sampling) — sample from the smallest set of tokens covering p probability mass.
  - `top_k` — sample from k most likely tokens.
  - `frequency_penalty` / `presence_penalty` — reduce repetition.
  - `stop sequences` — where to cut off.
  - `seed` — reproducibility (best-effort).
- **Logprobs** — token-level confidence. Useful for uncertainty estimation and eval.
- **Speculative decoding** — a small model drafts, a big model verifies. Free speedup.
- **Context window** — what it is, and the "lost in the middle" problem where information in the middle of a long context gets less attention.

### 8.6 Scaling laws & capabilities

- Chinchilla scaling — the parameters/data tradeoff.
- Test-time compute scaling — thinking longer improves reasoning; the newer axis of progress.
- Emergent capabilities and the debate about whether they're real or measurement artifacts.
- Model families and their tradeoffs: frontier vs mid vs small, open-weight vs closed.

### 8.7 The depth exercise

**Build a small GPT from scratch, once.** Andrej Karpathy's `nanoGPT` / "Let's build GPT" video is the canonical path. ~300 lines. Train it on a small text corpus.

You will never do this at work. Do it anyway. It converts LLMs from magic into machinery, and it's the single best answer to "do you actually understand what's under the API?"

**You do not need to train a real LLM.** That requires thousands of GPUs and millions of dollars. Anyone telling you a portfolio project should be "train your own LLM" is confusing you with a research lab.

---

## 9. Layer 6 — Using LLMs (The Actual Job)

**Goal: Mastery. This is what you get paid for.**

### 9.1 Working with model APIs

- The major providers and their APIs (Anthropic, OpenAI, Google, plus open-weight models via Together/Fireworks/Groq/Bedrock/Vertex).
- Message format: system / user / assistant roles, multi-turn structure.
- **Streaming** — SSE, handling partial responses, cancellation. Users perceive streaming as dramatically faster. Implement it.
- Token limits: input, output, and total context.
- Rate limits: RPM, TPM, concurrency. Handle 429s with exponential backoff + jitter.
- Retries, timeouts, circuit breakers, fallback models.
- Idempotency for expensive calls.
- Batch APIs — big cost savings for non-realtime workloads.
- **Prompt caching** — cache the static prefix of your prompt. Often the single largest cost win available. Learn each provider's caching semantics.
- Cost accounting: price per input token vs output token, and why output tokens are more expensive.

### 9.2 Prompt engineering (the real version)

Not "magic words." It's specification writing.

**Core techniques:**
- **Clear instructions** — be explicit about task, format, constraints, and audience. Ambiguity is the enemy.
- **Role/system prompts** — set context, persona, and rules once.
- **Few-shot prompting** — examples beat explanations. 2–5 well-chosen examples often outperform paragraphs of instruction.
- **Chain-of-thought** — ask for reasoning before the answer. Note: reasoning models do this natively; explicit CoT prompting can be redundant or harmful with them.
- **Structured output** — request JSON/XML; better, use the provider's structured output / JSON mode / tool-calling with a schema. Never parse free text when you can enforce a schema.
- **XML/Markdown delimiters** — separate instructions from data. Critical for injection resistance.
- **Prefilling the assistant turn** — force a format start.
- **Decomposition** — split a complex task into a chain of simple calls. Almost always beats one giant prompt.
- **Self-consistency** — sample multiple times, take the majority.
- **Reflection / self-critique** — have the model review its own output. Works, but costs a second call, and models are poor at catching their own errors in some domains.
- **Negative instructions are weak** — tell the model what to do, not what to avoid.
- **Put instructions after long context** in some cases; test it.

**Advanced patterns:**
- Prompt chaining / pipelines.
- Router prompts (classify then dispatch to specialized prompts).
- Ensembling (multiple prompts, aggregate).
- Meta-prompting (using a model to improve a prompt).
- Constitutional / rubric-guided self-revision.

**Discipline:**
- **Version your prompts.** In git, with tests. A prompt is code.
- **Never tune a prompt without an eval set.** Otherwise you're fixing one case and breaking three.
- Keep a prompt registry with metadata: model, version, eval score, date.
- Test across models — prompts don't transfer perfectly.

### 9.3 Structured output & function calling

- JSON mode vs constrained decoding vs tool schemas.
- Defining schemas with JSON Schema / Pydantic.
- Validation and repair loops (parse → validate → on failure, feed the error back).
- Nested and complex schemas — and why simpler schemas produce better reliability.
- Enums over free text for classification.

### 9.4 Model selection

Build a decision framework:

| Consideration | Questions to ask |
|---|---|
| Capability | Does the task need frontier reasoning or is it pattern matching? |
| Latency | Is this interactive or batch? |
| Cost | What's the per-request budget at expected volume? |
| Context | How much input does it need? |
| Modality | Text only? Images? Audio? |
| Privacy | Can data leave your infrastructure? |
| Determinism | Do you need reproducibility? |
| Deployment | Hosted API vs self-hosted open weights? |

**Model routing** — use a small cheap model for easy requests and escalate to a big one for hard ones. Classic production cost optimization. Learn it.

**Open vs closed weights:**
- Closed (API): best capability, no infra burden, per-token cost, data leaves your network, vendor dependency.
- Open (self-hosted): data control, fixed infra cost, customizable, requires GPU ops expertise, usually lags frontier capability.

### 9.5 Context management

- Context window budgeting: system prompt + history + retrieved docs + output reserve.
- Conversation history strategies: full history, sliding window, summarization, hierarchical summary.
- **Context rot / lost-in-the-middle** — long contexts degrade attention to middle content. More context is not automatically better.
- Compaction: summarize old turns to preserve budget.
- Structured memory: extract facts to a store rather than keeping raw transcripts.

### 9.6 Handling model failure modes

- **Hallucination** — confident fabrication. Mitigate with grounding (RAG), citations, "say I don't know" instructions, and verification passes. You cannot eliminate it.
- **Sycophancy** — agreeing with the user's wrong premise.
- **Instruction drift** — forgetting rules over long conversations.
- **Refusals** — false positives on benign requests.
- **Format violations** — mitigate with schemas.
- **Repetition loops** — mitigate with penalties and stop sequences.
- **Nondeterminism** — same input, different output. Design for it; don't fight it.

**Engineering principle: treat the model as an unreliable subsystem.** You would not call a flaky third-party API without retries, validation, timeouts, and fallbacks. Same here.

---

## 10. Layer 7 — RAG (Retrieval-Augmented Generation)

**Goal: Mastery. The most common production LLM pattern, and the one people do worst.**

RAG = retrieve relevant information, then generate an answer grounded in it. Simple concept. Endless implementation depth.

### 10.1 Why RAG

- Injects private/current knowledge without training.
- Reduces hallucination via grounding.
- Enables citations and auditability.
- Cheaper and faster to update than fine-tuning.
- Access control: retrieve only what this user may see.

### 10.2 The ingestion pipeline

**Step 1 — Loading**
- Sources: files, databases, APIs, web crawls, wikis, ticketing systems.
- Parsers per format. **PDF parsing is the usual quality bottleneck** — evaluate multiple parsers on your actual documents. Consider vision-model-based parsing for complex layouts (tables, multi-column, scanned).
- Preserve structure: headings, tables, lists, page numbers. Structure carries meaning.
- Capture metadata: source, URL, author, date, section, permissions.

**Step 2 — Chunking**

The decision that most determines RAG quality.

- **Fixed-size** — simple, dumb, splits mid-sentence. Baseline only.
- **Recursive character splitting** — split on paragraph → sentence → word. Sensible default.
- **Semantic chunking** — split where embedding similarity drops.
- **Document-structure-aware** — split on headings, sections. Usually best for structured docs.
- **Sentence-window** — embed single sentences, retrieve with surrounding context.
- **Parent-document / small-to-big** — embed small chunks, return the larger parent chunk to the model.
- **Late chunking** — embed the full document with a long-context embedder, then pool per chunk.
- **Contextual retrieval** — prepend an LLM-generated summary of the document context to each chunk before embedding. Substantially improves retrieval; costs an ingestion-time LLM call per chunk. Worth it.
- **Chunk overlap** — typically 10–20%, to avoid cutting concepts in half.

Tuning: chunk size (200–1000 tokens typical), overlap, and boundaries. **Test empirically.** There is no universal correct answer.

**Step 3 — Embedding**
- Choose an embedding model: dimension, max sequence length, domain fit, multilingual support, cost, and whether it's hosted or local.
- Check the MTEB leaderboard, but validate on *your* data. Leaderboard rank ≠ your use case.
- Batch your embedding calls.
- Normalize vectors if using dot product as cosine.
- **Version your embeddings.** Changing embedding models means re-indexing everything.

**Step 4 — Indexing**
- Store vectors + text + metadata.
- Index type: HNSW (fast, memory-hungry), IVF (partition-based), Flat (exact, small datasets only), ScaNN, DiskANN.
- Understand the recall/latency/memory tradeoff. `ef_search`, `M`, `nprobe` are knobs you should be able to explain.
- Quantization (PQ, binary, scalar) for memory reduction at some recall cost.

### 10.3 Vector databases

| Option | Best for |
|---|---|
| **pgvector** (Postgres) | Default choice. You already have Postgres. Handles millions of vectors fine. |
| Qdrant | Great filtering, Rust, self-host friendly |
| Weaviate | Built-in hybrid search |
| Milvus | Very large scale |
| Chroma | Local dev, prototyping |
| Pinecone | Fully managed, zero ops |
| Elasticsearch / OpenSearch | If you already run it; strong hybrid |
| FAISS | Library not database; in-memory, fast, no persistence layer |

**Advice:** start with pgvector. Migrate only when you have a measured reason. "We need a vector database" is usually premature.

### 10.4 Retrieval strategies

- **Dense retrieval** — embedding similarity. Good at semantics, weak on exact terms, IDs, and rare words.
- **Sparse retrieval** — BM25/keyword. Good at exact matches, weak on paraphrase.
- **Hybrid search** — combine both. **Almost always better than either alone.** Fuse with Reciprocal Rank Fusion (RRF) or weighted scores.
- **Metadata filtering** — filter by date, source, permission, type. Pre-filter vs post-filter matters for both correctness and performance.
- **Multi-vector retrieval** (ColBERT-style) — token-level matching, higher quality, higher cost.
- **Graph RAG** — build an entity/relationship graph; good for multi-hop and "connect the dots" questions.
- **Hierarchical retrieval** — retrieve summaries first, then drill into details.

### 10.5 Query processing

- **Query rewriting** — turn conversational input into a standalone search query. Essential for multi-turn chat.
- **Query expansion** — generate synonyms/variants, retrieve for each.
- **HyDE** (Hypothetical Document Embeddings) — generate a fake answer, embed *that*, search with it. Works surprisingly well.
- **Query decomposition** — split complex questions into sub-questions, retrieve for each.
- **Routing** — classify the query and send it to the right index or tool.
- **Step-back prompting** — ask a broader question first for context.

### 10.6 Post-retrieval

- **Reranking** — retrieve 50 candidates with a fast method, rerank with a cross-encoder, keep top 5. **This is the highest-ROI single improvement in most RAG systems.** Use a dedicated reranker model.
- **Context compression** — strip irrelevant sentences from retrieved chunks.
- **Deduplication** — near-duplicate chunks waste context.
- **Ordering** — put the most relevant chunks at the start and end of the context, not buried in the middle.
- **Relevance thresholding** — if nothing scores well, say "I don't know" instead of answering from noise.

### 10.7 Generation

- Prompt template that clearly separates instructions, retrieved context, and the user question.
- **Demand citations.** Chunk IDs in the output, mapped back to sources. Non-negotiable for trust.
- Instruct: answer only from context; if not present, say so.
- Guard against prompt injection *inside retrieved documents* — treat retrieved text as untrusted data, not instructions.
- Handle the no-results case explicitly.

### 10.8 RAG evaluation

Measure both halves separately or you'll never know what's broken.

**Retrieval metrics:**
- Recall@k — did we retrieve the right chunk at all? (If this is low, nothing downstream can save you.)
- Precision@k, MRR, NDCG.
- Hit rate.

**Generation metrics:**
- **Faithfulness / groundedness** — is the answer supported by the retrieved context?
- **Answer relevance** — does it address the question?
- **Context relevance** — was the retrieved context actually useful?
- Citation accuracy — do the citations point to the right source?
- Completeness.

**Method:** build a golden set of question → expected-answer → expected-source triples. Start with 50–100 examples. Generate synthetic ones with an LLM, then have a human review them. Run your eval on every change.

Tools: Ragas, DeepEval, TruLens, or roll your own. Rolling your own teaches you more.

### 10.9 Common RAG failure modes

| Symptom | Likely cause |
|---|---|
| Right doc never retrieved | Chunking too coarse/fine; wrong embedding model; no hybrid search |
| Right doc retrieved, wrong answer | Poor prompt; context lost in the middle; no reranking |
| Confident wrong answers | No grounding instructions; no "I don't know" path |
| Works on simple questions, fails on complex | No query decomposition; single-hop retrieval on multi-hop questions |
| Good in dev, bad in prod | Eval set doesn't reflect real queries |
| Slow | No caching; retrieving too many chunks; synchronous pipeline |
| Wrong user sees wrong data | No permission filtering at retrieval time — **security incident, not a bug** |

### 10.10 When NOT to use RAG

- Data fits comfortably in context → just put it in context.
- You need behavior/style change, not knowledge → fine-tune.
- You need exact structured queries → SQL, not vectors.
- Knowledge is stable, small, and general → the model already knows it.

---

## 11. Layer 8 — Agents & Tool Use

**Goal: Build systems that take actions, not just produce text. The fastest-growing area of the field.**

### 11.1 Definitions

- **Tool use / function calling** — the model requests a function call; your code executes it and returns the result.
- **Agent** — a loop: model decides an action, action executes, result feeds back, repeat until done.
- **Workflow** — predetermined sequence of LLM steps. Deterministic control flow.
- **Agent vs workflow:** workflows are predictable and debuggable; agents are flexible and unpredictable. **Default to workflows.** Use agents only when the task genuinely requires open-ended decision-making. Most "agent" projects should have been workflows.

### 11.2 Tool design (the actual skill)

Bad tools produce bad agents. This is where most agent projects fail.

- Clear, descriptive tool names.
- Descriptions written *for the model*, explaining when to use it and when not to.
- Minimal, well-typed parameters. Enums over free strings.
- **Return useful errors** — "invalid date format, expected YYYY-MM-DD" not "error 400". The model can recover from a good error message.
- Keep the tool count manageable. Too many tools degrades selection accuracy. Group or namespace them.
- Idempotent where possible.
- **Confirmation gates for destructive actions.** Never let an agent delete, send, or pay without a human check unless you're very sure.
- Return concise results — dumping 50KB of JSON into context is self-sabotage.

### 11.3 Agent architectures

- **ReAct** — reason, act, observe, repeat. The foundational pattern.
- **Plan-and-execute** — plan all steps first, then run them. More predictable, less adaptive.
- **Reflection** — critique output, revise.
- **Multi-agent** — orchestrator/worker, specialist agents, debate. Powerful but expensive and hard to debug. Justify it before you build it.
- **Router** — classify request, dispatch to the right handler.
- **Evaluator-optimizer** — one model generates, another critiques, loop.

### 11.4 Critical engineering concerns

- **Loop termination** — max iterations, max cost, max time, and a "stuck" detector. An unbounded agent loop is a runaway bill.
- **State management** — what does the agent remember between steps and between sessions?
- **Context growth** — every tool result adds tokens. Compact or summarize or you'll blow the window mid-task.
- **Error recovery** — tool failed; can the agent retry, use a different tool, or ask the user?
- **Human-in-the-loop** — approval gates, interruption, correction.
- **Observability** — full traces of every decision. **You cannot debug an agent without traces.**
- **Sandboxing** — code execution, file access, and network access must be isolated. Containers, restricted permissions, allowlists.
- **Cost controls** — hard budget caps per task.
- **Parallelism** — run independent tool calls concurrently.

### 11.5 Standards & protocols

- **MCP (Model Context Protocol)** — open standard for connecting models to tools and data sources. Increasingly the interoperability layer. Learn it: servers, clients, resources, tools, prompts.
- Computer use / browser automation — models controlling GUIs and browsers.
- Code execution environments as a tool.

### 11.6 Frameworks

- **LangGraph** — graph-based agent orchestration. Explicit state machines. Currently the most production-credible.
- **LangChain** — huge ecosystem, heavy abstraction. Useful for prototyping, often stripped out for production.
- **LlamaIndex** — strongest on RAG/data ingestion.
- **Provider-native agent SDKs** — increasingly good, less abstraction overhead.
- **CrewAI / AutoGen** — multi-agent, more experimental.
- **Pydantic AI** — type-safe, lightweight, pleasant.
- **DSPy** — programmatic prompt optimization; compiles prompts against metrics. Genuinely different idea, worth understanding.

**Strong recommendation:** build one agent from scratch — just a while loop, an API call, and a tool dispatcher — before touching any framework. It's maybe 150 lines. Then you'll know what the framework is hiding, and whether you need it.

### 11.7 Realistic expectations

Agents are unreliable at long-horizon tasks. Error compounds: 95% step accuracy over 20 steps is 36% task accuracy. Design for short horizons, checkpoints, and verification. Anyone promising fully autonomous agents for complex work is selling something.

---

## 12. Layer 9 — Evaluation (The Skill Nobody Has)

**Goal: Mastery. This is the highest-leverage, most under-supplied skill in AI engineering.**

Most people building with LLMs evaluate by vibes. "Looks better to me." That doesn't scale, doesn't survive a model upgrade, and doesn't convince anyone. If you become the person who can *measure* AI quality, you become indispensable.

### 12.1 Why evals are everything

- Without evals you cannot tell improvement from regression.
- You cannot safely change models, prompts, or retrieval settings.
- You cannot debug — you only have anecdotes.
- You cannot make a case to stakeholders.
- **Evals are the test suite of AI engineering.** You wouldn't ship code without tests.

### 12.2 Types of evaluation

**By method:**
- **Code-based / deterministic** — exact match, regex, JSON schema validation, does-it-compile, does-it-run. Cheapest and most reliable. Use wherever possible.
- **Statistical** — BLEU, ROUGE, METEOR, semantic similarity. Weak proxies for quality; mostly legacy.
- **LLM-as-judge** — a model grades outputs against a rubric. Flexible, scalable, imperfect.
- **Human evaluation** — the gold standard, the bottleneck. Use for calibration and high-stakes decisions.

**By scope:**
- **Unit evals** — a single component (retrieval, one prompt, one tool).
- **End-to-end evals** — the whole system on a realistic task.
- **A/B tests** — real users, real traffic, statistical significance.
- **Regression suites** — a set of known cases that must never break.
- **Adversarial / red team** — deliberately try to break it.

### 12.3 Building an eval set

1. **Start with real data.** Log real user queries. Twenty real examples beat two hundred imagined ones.
2. **Include failure cases.** Every bug you fix becomes a test case. This is how the suite gets valuable.
3. **Cover the distribution:** easy, hard, edge, adversarial, out-of-scope, ambiguous.
4. **Synthetic augmentation** — generate variations with an LLM, then human-review them.
5. **Golden answers** — write the expected output or rubric. Hardest part; do it anyway.
6. **Version control it.** JSONL in git.
7. **Size:** start at 30–50. Grow to a few hundred. Quality > quantity. A curated 100 beats a sloppy 5000.

### 12.4 LLM-as-judge, done properly

- **Give it a rubric**, not "rate 1–10." Specific criteria with definitions.
- **Prefer binary or few-category judgments** over fine-grained scores. Models are bad at calibrated numeric scales.
- **Pairwise comparison** (A vs B) is more reliable than absolute scoring.
- **Ask for reasoning before the verdict.**
- **Control for position bias** — swap A/B order and average.
- **Control for length bias** — judges favor longer answers. Watch for it.
- **Use a strong model as judge**, ideally a different family than the one being judged.
- **Validate the judge against human labels.** Measure agreement (Cohen's kappa). If your judge doesn't agree with humans, your metric is fiction.

### 12.5 Metrics that matter

**Task quality:** accuracy, faithfulness/groundedness, relevance, completeness, coherence, format compliance, instruction adherence, tone/style.

**Retrieval:** recall@k, precision@k, MRR, NDCG.

**Agents:** task completion rate, steps to completion, tool-selection accuracy, error-recovery rate, cost per task.

**Operational:** latency (p50/p95/p99), time-to-first-token, tokens/second, cost per request, error rate, timeout rate, cache hit rate.

**Safety:** refusal appropriateness (both false refusals and missed refusals), toxicity, PII leakage, jailbreak resistance.

**Business:** user satisfaction, thumbs up/down rate, task success as users define it, escalation-to-human rate, retention.

### 12.6 Eval infrastructure

- Run evals in CI. Block merges on regressions.
- Store results over time; plot trends.
- Tie every eval run to a prompt version, model version, and config hash.
- Dashboard it so non-engineers can see quality.
- Sample production traffic continuously into evals (online eval).
- **Error analysis** — the actual work: read failures, categorize them, count categories, fix the biggest bucket. Do this manually and often. It is more valuable than any dashboard.

Tools: Braintrust, LangSmith, Langfuse, Phoenix/Arize, Ragas, DeepEval, promptfoo, OpenAI Evals, Inspect. Learn the *concepts*; tools churn.

### 12.7 Benchmarks (context, not goals)

Know the big public ones (MMLU, GPQA, SWE-bench, HumanEval, MATH, ARC-AGI, HELM, LMArena) so you can read model announcements critically. But: **public benchmarks are contaminated, gamed, and rarely predict performance on your task.** Your eval set is the only one that matters for your product.

---

## 13. Layer 10 — Fine-Tuning & Model Adaptation

**Goal: Know how, and more importantly know *when* — which is less often than people think.**

### 13.1 The decision tree

Try in this order:
1. **Better prompting** — free, instant, usually enough.
2. **Few-shot examples** — cheap, often solves "wrong format/style."
3. **RAG** — for knowledge gaps.
4. **Better model** — sometimes the simplest fix.
5. **Prompt optimization** (DSPy-style) — automated.
6. **Fine-tuning** — last, when you need consistent behavior, specialized style/format, domain vocabulary, or you're distilling a big model into a small cheap one.

**Fine-tuning is good at:** style, format, tone, task-specific behavior, latency/cost reduction via smaller models, domain-specific language.

**Fine-tuning is bad at:** teaching new facts (use RAG), fixing reasoning ability, and anything where your data is small or messy.

### 13.2 Methods

- **Full fine-tuning** — update all weights. Expensive, needs serious GPU memory.
- **LoRA** — Low-Rank Adaptation. Freeze base weights, train small adapter matrices. ~1% of parameters, near-full quality. The standard.
- **QLoRA** — LoRA on a quantized base model. Lets you fine-tune large models on a single consumer/prosumer GPU.
- **Other PEFT methods** — prefix tuning, prompt tuning, adapters, IA³.
- **Instruction tuning (SFT)** — teach a base model to follow instructions.
- **Preference tuning** — DPO, ORPO, KTO, and RLHF. Align to human preferences. DPO is the practical default now — simpler than PPO-based RLHF.
- **Distillation** — train a small model on a big model's outputs. Excellent cost-reduction play.
- **Continued pretraining** — inject deep domain knowledge on a large domain corpus. Expensive, rarely justified.

Key LoRA hyperparameters: rank `r` (capacity), `alpha` (scaling), target modules (which layers get adapters), dropout.

### 13.3 Data for fine-tuning

**This is 90% of success.**

- Quality > quantity. 500 excellent examples beat 50,000 mediocre ones.
- Consistency: same format, same style, same conventions throughout.
- Coverage: representative of real inputs.
- Deduplicate. Aggressively.
- Hold out a test set that the model never sees.
- Watch for **catastrophic forgetting** — the model gets better at your task and worse at everything else. Mix in general data if this matters.
- Common formats: chat JSONL (messages arrays), instruction/response pairs, preference triples (prompt, chosen, rejected).

### 13.4 Practicalities

- Tooling: Hugging Face `transformers` + `peft` + `trl`, Axolotl, Unsloth (fast, memory-efficient), LLaMA-Factory, or hosted fine-tuning APIs.
- Hardware: QLoRA on a 7B model fits on a 24GB GPU. Larger models need A100/H100-class hardware or cloud rental.
- Watch: training loss vs validation loss, overfitting after 2–3 epochs (common with small datasets), learning rate (much lower than pretraining).
- Deployment: merge adapters or serve them dynamically. Multiple LoRAs can be served from one base model.
- **Always evaluate against the un-fine-tuned baseline.** People routinely fine-tune, feel accomplished, and never check whether it actually beat a good prompt.

### 13.5 Cost reality

Fine-tuning is not just training cost. It's: data collection, data cleaning, training runs, eval, hosting, monitoring, and **re-doing all of it when a better base model ships in four months.** Budget for the maintenance, not just the experiment.

---

## 14. Layer 11 — Inference, Serving & Performance

**Goal: Make it fast and cheap. Matters more as you get senior.**

### 14.1 Latency

- **Time to First Token (TTFT)** — dominated by prefill and network. What users feel as "responsiveness."
- **Tokens per second (TPS)** — decode speed. What users feel as "reading pace."
- **Total latency** — end to end.
- Optimizations: streaming (perceptual, huge), prompt caching, shorter prompts, smaller models, parallel calls, speculative decoding, region selection, connection reuse, and doing work before the user asks (prefetch).

### 14.2 Throughput & serving (self-hosted)

- **Continuous batching** — the key throughput technique in modern servers.
- **PagedAttention** — efficient KV cache memory management (vLLM's core innovation).
- Serving engines: **vLLM** (default choice), SGLang, TensorRT-LLM (NVIDIA, fastest, fiddly), TGI, llama.cpp / Ollama (local, CPU-friendly, GGUF).
- Tensor parallelism, pipeline parallelism for big models.
- KV cache sizing and how it limits concurrency.

### 14.3 Quantization

- FP32 → FP16/BF16 → INT8 → INT4.
- Methods: GPTQ, AWQ, GGUF quant levels, bitsandbytes.
- Tradeoff: memory and speed vs quality. INT8 is usually near-lossless; INT4 is noticeable but often acceptable.
- Know how to measure the quality loss instead of assuming it.

### 14.4 Cost optimization (a real job responsibility)

Techniques in rough order of impact:
1. **Prompt caching** — cache static prefixes. Often 50–90% savings on repeated system prompts.
2. **Model routing** — cheap model for easy tasks.
3. **Semantic caching** — cache answers to similar questions.
4. **Shorter prompts** — audit for bloat; people ship 4000-token system prompts that should be 400.
5. **Cap output length** — output tokens cost more.
6. **Batch API** — for anything non-realtime.
7. **Distillation** — replace an expensive model with a fine-tuned small one.
8. **Deduplicate requests** — don't call twice for the same thing.
9. **Fail fast** — validate input before spending tokens.

Track: cost per request, cost per user, cost per resolved task. **Set budget alerts before you need them.**

### 14.5 Hardware literacy

- GPU memory: model weights + KV cache + activations. Estimate before you deploy.
- Rough rule: FP16 model needs ~2GB VRAM per billion parameters, plus KV cache.
- Memory bandwidth is usually the decode bottleneck, not FLOPs.
- Know the tiers: consumer (4090), datacenter (A100, H100, and successors), and the cloud alternatives (TPUs, Inferentia).

---

## 15. Layer 12 — LLMOps, Observability & Cost

**Goal: Run AI systems in production without flying blind.**

### 15.1 Observability

- **Tracing** — every request as a trace with spans: prompt construction, retrieval, model call, tool calls, parsing. **Non-negotiable for agents.**
- Log: input, output, model, prompt version, tokens, latency, cost, user, session, and outcome.
- OpenTelemetry-compatible tooling: Langfuse, LangSmith, Phoenix, Braintrust, Helicone, W&B Weave.
- Correlate traces with user feedback.
- Sample and review real traffic weekly. Read actual conversations. You will find things no dashboard shows.

### 15.2 Monitoring & alerting

- Latency percentiles, not averages. p95 and p99 are the user experience.
- Error rates by type: rate limits, timeouts, parse failures, tool failures, refusals.
- Cost per hour/day, with anomaly alerts.
- Quality metrics from online evals.
- Drift: input distribution changes, output quality changes.
- Model version changes — providers update models; behavior shifts under you. Pin versions where you can, and re-run evals when they change.

### 15.3 Deployment practice

- **Prompt versioning** — in git, with eval scores attached.
- Feature flags for model/prompt rollout.
- Canary deploys and gradual rollout.
- Shadow mode — run the new version alongside the old, compare, don't serve.
- Instant rollback path.
- Reproducibility: pin model versions, log configs.

### 15.4 Reliability engineering

- Timeouts on every external call.
- Retries with exponential backoff and jitter.
- Circuit breakers on failing providers.
- **Fallback chains** — provider A fails → provider B → cached/degraded response.
- Graceful degradation: a worse answer beats an error page.
- Queue long tasks; don't hold HTTP connections open for two minutes.
- Idempotency keys for expensive operations.

### 15.5 The feedback loop

```
Ship → Log → Review failures → Categorize → Add to eval set →
Fix → Verify against evals → Ship
```

This loop *is* the job. Everything else is setup.

---

## 16. Layer 13 — AI System Design

**Goal: Design AI features end to end. Heavily interview-tested.**

### 16.1 The design process

1. **Clarify requirements** — who uses it, what does success mean, what's the volume, what's the latency budget, what's the cost budget, what accuracy is acceptable, what happens when it's wrong?
2. **Question whether AI is needed at all.** Rules, search, or a classifier may be better. Saying this in an interview is a strong signal.
3. **Pick the pattern** — direct call, RAG, workflow, agent, hybrid.
4. **Design data flow** — ingestion, storage, indexing, retrieval, generation, output.
5. **Design for failure** — what if the model is wrong, slow, down, or expensive?
6. **Define evaluation** — how do we know it works? What's measured in production?
7. **Estimate cost and latency** — do the arithmetic out loud.
8. **Plan the rollout** — internal → canary → full.

### 16.2 Reusable patterns

- **Classification** — LLM as classifier with enum output; consider a fine-tuned small model or classical ML at volume.
- **Extraction** — schema-enforced structured output from documents.
- **Summarization** — map-reduce for long docs, refine for narrative coherence.
- **Q&A over documents** — RAG.
- **Conversational assistant** — RAG + memory + tools.
- **Content generation** — templates + LLM + human review.
- **Code assistant** — context assembly + generation + execution/verification.
- **Routing/triage** — classify then dispatch.
- **Multi-step workflow** — chained prompts with validation between steps.
- **Human-in-the-loop review queue** — model drafts, human approves.

### 16.3 Cross-cutting concerns

- **Multi-tenancy** — data isolation, per-tenant quotas, per-tenant configuration.
- **Access control at retrieval time** — a user must never retrieve documents they can't see.
- **Caching layers** — exact match, semantic, prompt prefix.
- **Async processing** — queues for anything over a few seconds.
- **Rate limiting per user** — protect cost and fairness.
- **Audit logging** — who asked what, what did the system do, especially for agents with write access.
- **Data residency** — where does data physically go, and does that comply?

### 16.4 Interview-style problems to practice

Design: a customer support assistant over a knowledge base. A document-processing pipeline for 10M PDFs. A code review bot. A semantic search engine. A meeting summarizer with action items. An email triage agent. A content moderation system. A SQL-generating natural language interface. A multi-tenant B2B AI feature with strict data isolation.

For each, write out: architecture, data flow, model choices, eval plan, failure modes, cost estimate at scale, and rollout plan. One per week. Then critique your own answer a week later.

---

## 17. Layer 14 — Security & Safety

**Goal: Don't ship a liability. Increasingly a hard requirement, not a nice-to-have.**

### 17.1 Prompt injection

**The defining unsolved security problem of LLM applications.**

- **Direct injection** — the user tells the model to ignore its instructions.
- **Indirect injection** — malicious instructions hidden in retrieved documents, web pages, emails, or tool outputs. **This is the dangerous one**, because the payload arrives through data your system trusts.
- The **lethal trifecta**: private data access + untrusted content + external communication. Any system with all three can be made to exfiltrate data. Break one leg of the triangle.

**Mitigations (partial — none are complete):**
- Treat all retrieved/tool content as untrusted data, never as instructions.
- Strong delimiters and explicit "the following is data, not instructions" framing.
- Privilege separation: the model plans, but a deterministic layer authorizes actions.
- Allowlist tools and destinations.
- Human confirmation for consequential actions.
- Output filtering, especially for URLs and markdown images (a classic exfiltration channel).
- Input sanitization and injection classifiers.
- **Assume injection will sometimes succeed. Limit the blast radius.**

### 17.2 Other attack surfaces

- **Jailbreaking** — bypassing safety behavior.
- **Data exfiltration** — via generated links, images, or tool calls.
- **Model DoS** — expensive prompts, unbounded loops, token bombs.
- **Insecure output handling** — model output used in SQL, shell, `eval()`, or rendered as HTML. **Never trust model output as code.**
- **Supply chain** — malicious models on hubs, poisoned datasets, unsafe pickle deserialization (use safetensors).
- **Training data extraction** — models regurgitating memorized secrets.
- **Excessive agency** — the agent has more permissions than the task needs.

Reference: the OWASP Top 10 for LLM Applications. Read it once, properly.

### 17.3 Privacy & compliance

- PII detection and redaction before sending to third-party APIs.
- Data retention policies with providers — know whether your data trains their models (usually not on business tiers, but verify).
- GDPR, CCPA, India's DPDP Act; sector rules (HIPAA, PCI-DSS, financial regulations).
- The EU AI Act — risk categories, transparency obligations, timelines.
- Right to deletion — can you actually delete a user's data from your vector index?
- Consent and disclosure — users should know they're talking to AI.

### 17.4 Responsible AI

- **Bias** — models reflect training data. Test across demographic groups where relevant.
- **Fairness** — differential performance across user populations.
- **Transparency** — disclose AI involvement, cite sources, expose uncertainty.
- **Human oversight** — for consequential decisions (hiring, credit, medical, legal), a human must be in the loop and must be able to override.
- **Content provenance** — watermarking, C2PA.
- **Misuse** — think about how your system could be abused before someone shows you.
- **Environmental cost** — real, worth considering in model choice.

### 17.5 Guardrails

- Input: injection detection, PII scrubbing, topic filtering, length limits.
- Output: schema validation, toxicity/PII checks, groundedness checks, policy compliance.
- Tools: provider moderation endpoints, NeMo Guardrails, Guardrails AI, Llama Guard, or custom classifiers.
- **Layer guardrails; don't rely on prompting alone.** Prompted rules are suggestions, not enforcement.

---

## 18. Layer 15 — Multimodal & Beyond Text

**Goal: Awareness now, depth if your domain needs it.**

### 18.1 Vision

- Vision-language models (VLMs): image input to a text model.
- Use cases: document understanding, chart/diagram reading, screenshot analysis, OCR replacement, visual QA, UI automation.
- Practicalities: image resolution and token cost, multiple images per request, detail settings.
- **VLM-based document parsing is now often better than traditional PDF extraction** for complex layouts. Genuinely useful.
- Image generation: diffusion models, prompt structure, ControlNet, inpainting, LoRAs for style.
- Classical CV literacy: object detection (YOLO), segmentation (SAM), image embeddings (CLIP).

### 18.2 Audio

- Speech-to-text: Whisper and successors. Diarization, timestamps, streaming transcription.
- Text-to-speech: quality, latency, voice cloning, and the ethics thereof.
- Realtime speech-to-speech APIs — low-latency voice agents. Growing fast.
- Use cases: meeting notes, voice agents, call analytics, accessibility.

### 18.3 Video

- Video understanding: frame sampling, temporal reasoning, long-video summarization.
- Video generation: emerging, expensive, rapidly improving.

### 18.4 Multimodal RAG

- Embedding images and text into a shared space (CLIP-style).
- Retrieving figures, tables, and diagrams alongside text.
- Screenshot-based retrieval for documents (embed page images directly — often outperforms text extraction).

### 18.5 Other

- Structured/tabular reasoning: text-to-SQL, spreadsheet manipulation.
- Time series with foundation models.
- Scientific domains: protein models, molecular models.

---

## 19. Layer 16 — Optional Depth: Training Models

**Goal: Only if you're targeting research or infra roles at a lab. Otherwise: read once, move on.**

**You do not need this to be an AI engineer.** Included for completeness and because you asked for everything.

### 19.1 What pretraining actually involves

- Data: trillions of tokens; collection, filtering, deduplication, quality classification, decontamination against benchmarks, licensing.
- Compute: thousands of GPUs for weeks. Millions of dollars.
- Distributed training: data parallelism, tensor parallelism, pipeline parallelism, ZeRO/FSDP, 3D parallelism.
- Infrastructure: checkpointing, fault tolerance (hardware *will* fail mid-run), monitoring, throughput optimization (MFU).
- Stability: loss spikes, divergence, numerical precision issues.
- Scaling law experiments to choose hyperparameters before the big run.

### 19.2 Post-training

- SFT on curated instruction data.
- Reward modeling.
- RLHF with PPO; or DPO and simpler preference methods.
- RL on verifiable rewards for reasoning/code/math.
- Constitutional AI / RLAIF — using AI feedback against a set of principles.
- Red-teaming and safety training.

### 19.3 Research-adjacent skills

- Reading papers efficiently: abstract → figures → results → method → related work.
- Reproducing results.
- Running clean ablations.
- Statistical rigor: error bars, multiple seeds, significance.
- Writing up findings clearly.
- Interpretability: probing, activation patching, sparse autoencoders, circuits.

### 19.4 The one thing you *should* do

Implement a small transformer and train it on a toy corpus. Once. `nanoGPT` scale. This is the depth exercise from Layer 5. It takes a weekend and permanently changes how you think about the models you use.

---

## 20. The Tool Stack — What to Learn, What to Skip

### Learn properly

| Category | Tool | Why |
|---|---|---|
| Language | **Python** | The lingua franca |
| Package mgmt | **uv** | Fast, modern, replacing pip/poetry |
| Web framework | **FastAPI** | Default for AI services |
| Validation | **Pydantic** | Everywhere in the ecosystem |
| Deep learning | **PyTorch** | Standard |
| Model hub | **Hugging Face** | transformers, datasets, hub, peft, trl |
| Vector store | **pgvector** | Start here; Postgres you already run |
| Serving | **vLLM** | Standard for self-hosted inference |
| Local models | **Ollama** | Fastest way to run models locally |
| Orchestration | **LangGraph** | Explicit, debuggable agent graphs |
| Observability | **Langfuse** or LangSmith | Traces are mandatory |
| Evals | **promptfoo** or Braintrust | Or build your own first |
| Containers | **Docker** | Non-negotiable |
| CI | **GitHub Actions** | Non-negotiable |
| Cloud | **One of AWS/GCP/Azure** | Depth in one beats surface in three |
| Notebooks | **Jupyter** | For exploration only, never production |

### Learn if needed

Qdrant/Weaviate/Pinecone, LlamaIndex, DSPy, Unsloth/Axolotl, Ray, Airflow/Dagster, Terraform, Kubernetes, Modal/RunPod (GPU rental), Gradio/Streamlit (demos), Next.js (if you build UIs).

### Skip (for now)

- Kubernetes, until you actually need it.
- Every framework that launched last month with a lot of stars and no production users.
- Certifications. Nobody is hiring on a certificate.
- Building your own vector database.
- Building your own agent framework "for the portfolio." (Building one to *learn* is fine; shipping it as your differentiator is not.)

### Tool churn warning

Half of the specific tools in this document will be different in two years. **Learn the concepts, not the APIs.** Someone who understands retrieval, chunking, and reranking can pick up any vector database in a day. Someone who only knows one library's function names has nothing transferable.

---

## 21. Project Ladder

Projects, in increasing order of difficulty and signal. Each one should be **deployed, documented, and evaluated**. A GitHub repo with a README that shows metrics beats ten notebooks.

### Tier 1 — Foundations (weeks 1–4)

1. **CLI chat client** — raw API calls, streaming, conversation history, token counting, cost tracking. No frameworks. Understand the primitives.
2. **Structured extraction service** — upload a document, get validated JSON out. Schema enforcement + repair loop.
3. **Text classifier comparison** — same task solved three ways: regex/rules, scikit-learn, LLM. Compare accuracy, latency, and cost. **Write up which won and why.** This project alone demonstrates judgment most candidates lack.

### Tier 2 — Core competence (weeks 5–12)

4. **RAG system, done properly** — full pipeline: ingestion, chunking, hybrid search, reranking, citations, "I don't know" handling. Deployed. **With an eval set and reported metrics.** This is the single most important project on this list.
5. **Eval harness** — a reusable framework: dataset management, multiple graders (code + LLM judge), CI integration, results dashboard. Wildly underrated as a portfolio piece.
6. **Agent from scratch** — no framework. Loop, tool dispatch, error handling, termination conditions, cost caps, full tracing. ~200 lines. Then rewrite it in LangGraph and write up the comparison.

### Tier 3 — Differentiation (months 4–6)

7. **Multi-step workflow application** — a real task decomposed into stages, with validation between each, retries, and partial-failure recovery.
8. **Fine-tuned small model** — take a task an expensive model does well, generate training data from it, fine-tune a small model with LoRA, and show the cost/quality tradeoff with numbers. Distillation in miniature.
9. **Production-grade AI service** — auth, rate limiting, queuing, caching (exact + semantic), observability, cost dashboards, fallback chains, canary deploys. The "I can actually ship" project.
10. **nanoGPT reproduction** — the depth-signal project.

### Tier 4 — Senior signal (months 7+)

11. **Multimodal pipeline** — document understanding with a VLM, compared against traditional parsing, with quality metrics.
12. **Self-hosted inference deployment** — vLLM on a rented GPU, benchmarked for throughput and latency, quantization comparison, cost-per-token analysis vs API pricing.
13. **Red-team report** — take a public or your own AI application, systematically attempt prompt injection and jailbreaks, document findings and mitigations. Rare, valuable, and shows security awareness.
14. **Open-source contribution** — fix bugs or add features in a real AI library. Being in the commit history of a tool people use is stronger signal than any solo project.

### What makes a project count

| Weak | Strong |
|---|---|
| Notebook on GitHub | Deployed service with a URL |
| "It works" | "94% recall@5, p95 latency 1.2s, $0.003/request" |
| Tutorial reproduction | Something with a decision you had to make |
| No README | README with architecture diagram, design tradeoffs, and results |
| Untested | Eval suite in CI |
| Happy path only | Documented failure modes and how they're handled |
| Follows the framework's defaults | Explains why those defaults were wrong for this case |

**Three excellent projects beat fifteen mediocre ones.** Delete the mediocre ones from your GitHub.

---

## 22. Month-by-Month Study Plan

Assumes ~15–20 hours/week alongside a job. Compress if full-time.

### Month 1 — Foundations & first contact
- Python typing, async, Pydantic, FastAPI refresher.
- Docker + deploy something to a cloud run service.
- Raw LLM API calls: streaming, retries, token counting, cost tracking.
- Prompt engineering fundamentals. Read the provider docs properly — all of them.
- **Ship:** Project 1 (CLI chat client), Project 2 (structured extraction).

### Month 2 — Math, ML literacy, embeddings
- Linear algebra + probability essentials (3Blue1Brown; do the exercises).
- scikit-learn: train a few classifiers, learn the metrics cold.
- Embeddings: generate them, visualize them with UMAP, build a similarity search by hand with numpy.
- **Ship:** Project 3 (classifier comparison, three approaches).

### Month 3 — RAG, seriously
- Chunking strategies; test at least four on the same corpus.
- pgvector setup, hybrid search with BM25 + vectors, RRF fusion.
- Reranking with a cross-encoder.
- Citations and grounding.
- **Ship:** Project 4 (RAG v1, deployed).

### Month 4 — Evaluation
- Build a golden dataset from real queries.
- Code-based graders + LLM-as-judge with a rubric.
- Measure retrieval and generation separately.
- Wire evals into CI.
- Go back and improve the RAG system using the evals. Document the before/after.
- **Ship:** Project 5 (eval harness), Project 4 v2 with metrics.

### Month 5 — Deep learning & transformers
- PyTorch: training loop, a small model end to end.
- Transformer architecture study: attention, positional encoding, KV cache.
- **Ship:** Project 10 (nanoGPT). Yes, out of order — do it while the theory is fresh.

### Month 6 — Agents & tools
- Tool design principles.
- Build an agent from scratch, then with LangGraph.
- MCP: build a server and a client.
- Tracing and cost caps.
- **Ship:** Project 6 (agent, both versions, with write-up).

### Month 7 — Production engineering
- Observability stack: tracing, dashboards, alerting.
- Caching (exact + semantic), fallback chains, circuit breakers.
- Queues for long-running work.
- Load testing and latency profiling.
- **Ship:** Project 9 (production-grade service).

### Month 8 — Fine-tuning & inference
- LoRA/QLoRA with Unsloth or Axolotl on a rented GPU.
- Data curation for fine-tuning.
- vLLM deployment, quantization comparison, throughput benchmarks.
- **Ship:** Project 8 (distillation with cost/quality numbers), Project 12.

### Month 9 — Security, safety, system design
- OWASP LLM Top 10.
- Guardrails implementation.
- Red-team your own projects.
- One system design problem per week, written out.
- **Ship:** Project 13 (red-team report).

### Month 10–12 — Depth, visibility, and job search
- Pick a specialization: RAG at scale, agents, evals, inference optimization, or safety.
- Open-source contributions.
- Write and publish: 3–5 technical posts on things you actually learned the hard way.
- Interview prep: DSA maintenance, system design practice, project deep-dive rehearsal.
- Apply. Continuously. Not in a single batch at the end.

**Reality check:** this plan is aggressive. Twelve months at this intensity makes you genuinely employable in AI engineering *if you already have software engineering fundamentals*. If you're starting from zero programming, double it.

---

## 23. Portfolio, Resume & Public Presence

### GitHub

- **Pin 3–5 repos. Hide or delete the rest.** Curation is signal.
- Every pinned repo needs: a real README (what, why, architecture, results, how to run), clean commit history, tests, CI badge, and a live demo link if applicable.
- Include an architecture diagram. Costs an hour, dramatically improves perceived quality.
- **Report metrics.** Numbers separate engineers from enthusiasts.

### Resume

- Lead with impact and numbers: "cut inference cost 62% via model routing and prompt caching," not "worked with LLMs."
- Name specific technologies, but only ones you can be grilled on.
- One page for under 8 years of experience. Yes, one.
- Tailor to the posting. Match their vocabulary — many pipelines filter on keywords.
- Include a link to your best project and make sure it loads.

### Writing

- Write about problems you solved, with the failed attempts included. Failure narratives are more credible and more useful than success narratives.
- Good topics: "we tried X chunking strategies, here's what our eval showed," "why we removed our agent framework," "what our RAG system got wrong and how we found out."
- Consistency beats brilliance. Six solid posts over a year is plenty.

### Community

- Contribute to open-source AI tooling.
- Answer questions in Discord/forums for tools you use.
- Attend or present at meetups.
- Build in public — regular short updates outperform occasional long ones.

### Networking

- Most roles come through people, not portals. Uncomfortable but true.
- Engage genuinely with practitioners' work before you need anything from them.
- Referrals dramatically raise your response rate. Ask, politely, after you've built some relationship.

---

## 24. Interview Preparation

### Typical loop

1. Recruiter screen — motivation, background, compensation range.
2. Technical screen — coding, usually practical rather than pure algorithms.
3. Take-home or live build — construct something small with an LLM.
4. System design — design an AI feature end to end.
5. Deep dive — walk through a project you built, in detail, under questioning.
6. Behavioral / culture — collaboration, judgment, handling failure.

### Coding

- Data structures and algorithms still show up. Keep them warm: arrays, strings, hashmaps, two pointers, sliding window, binary search, trees, graphs, DP basics. Consistent light practice beats cramming.
- Practical coding is more common in AI roles: parse this data, call this API, handle these errors, write these tests.
- Know your Python cold, including async.

### AI-specific questions you must be able to answer

- Explain attention to a non-expert. Then to an expert.
- What's the difference between RAG and fine-tuning? When would you choose each?
- Your RAG system returns irrelevant results. Walk me through debugging it.
- How do you evaluate an LLM application?
- What is prompt injection and how do you defend against it?
- How do you reduce cost in an LLM application?
- Temperature vs top_p — what do they do?
- How does the KV cache work and why does it matter?
- When would you *not* use an LLM?
- Your model was upgraded and quality dropped. What now?
- How do you handle nondeterminism in tests?

### System design

Practice out loud with a timer. Structure: clarify → propose → deep dive → tradeoffs → failure modes → evaluation → cost. **Always ask about scale, latency, and budget before designing.** Always mention evaluation — most candidates forget, and interviewers notice.

### Project deep dive

For each portfolio project, be ready with:
- Why you built it, and the alternatives you rejected.
- The hardest bug and how you found it.
- Something you'd redo differently.
- Concrete numbers on performance and cost.
- What broke in production (or would, at scale).

Rehearse this. Vague answers on your own project are fatal.

### Behavioral

STAR format. Have stories ready for: a technical disagreement, a project that failed, a time you changed your mind on evidence, a time you shipped something under pressure, and a time you had to tell someone bad news.

### Questions to ask them

- How do you evaluate AI quality? (If they say "we eyeball it," that tells you a lot.)
- What's your prompt/model versioning process?
- How do you decide between building and buying?
- What's the biggest AI-related outage you've had?
- How much of the work is AI vs regular backend?

---

## 25. Learning Resources

### Foundational

- **3Blue1Brown** — Linear Algebra, Calculus, and Neural Networks series. Best conceptual math content available.
- **Andrej Karpathy — "Neural Networks: Zero to Hero"** — the single best deep-learning-from-scratch resource. Includes building GPT.
- **fast.ai — Practical Deep Learning for Coders** — top-down, practical.
- **Hugging Face courses** — NLP, LLM, Agents, Deep RL. Free, current, hands-on.
- **CS231n (vision), CS224n (NLP)** — Stanford, free lectures.

### Books

- *Designing Machine Learning Systems* — Chip Huyen. Production ML thinking.
- *AI Engineering* — Chip Huyen. Closest thing to a textbook for this exact role.
- *Hands-On Machine Learning* — Géron. The classical ML standard.
- *Build a Large Language Model (From Scratch)* — Raschka.
- *Designing Data-Intensive Applications* — Kleppmann. Not AI, but essential systems thinking.
- *Deep Learning* — Goodfellow et al. Reference, not a read-through.

### Documentation (read these properly, they're better than most courses)

- Anthropic docs and cookbook — prompt engineering, tool use, agents, MCP.
- OpenAI docs and cookbook.
- Hugging Face transformers/peft/trl docs.
- LangGraph and LlamaIndex docs.
- vLLM docs.

### Papers worth reading

Foundational: *Attention Is All You Need*; BERT; GPT-2/GPT-3; InstructGPT (RLHF); Chinchilla (scaling laws).

Practical: RAG (Lewis et al.); ReAct; Chain-of-Thought; LoRA; QLoRA; DPO; Toolformer; Self-Consistency; FlashAttention; Lost in the Middle.

**How to read:** abstract, figures, results, then method. Most papers deserve 15 minutes, not two hours.

### Staying current

- Follow practitioner blogs over hype accounts.
- Model provider engineering blogs.
- Read release notes and model cards for models you use.
- **Limit intake.** One hour a week of reading, the rest building. The field produces more content than anyone can consume, and consumption feels like progress while producing none.

---

## 26. Anti-Patterns — How People Waste a Year

1. **Tutorial hell.** Twelve courses, zero deployed projects. Stop after two and build.
2. **Math-first paralysis.** Six months on linear algebra "before starting." You will quit before you build anything.
3. **Framework worship.** Learning LangChain instead of learning LLMs. Frameworks change; principles don't.
4. **Vibes-based development.** No evals. You cannot improve what you don't measure.
5. **Prompt fiddling.** Endlessly tweaking wording with no test set. You're doing randomized search with a sample size of one.
6. **Notebook-only work.** Nothing deployed, nothing monitored, nothing real.
7. **Chasing every new model/tool.** Novelty consumption disguised as learning.
8. **Building an agent when a workflow would do.** Complexity you'll spend months debugging.
9. **Fine-tuning as the first resort.** Usually a prompt problem. Expensive lesson.
10. **Ignoring cost until the bill arrives.** Learn cost accounting early.
11. **Skipping software engineering.** The strongest AI engineers are strong engineers who learned AI. Not the reverse.
12. **Portfolio bloat.** Twenty half-finished repos signals worse than three finished ones.
13. **Applying only when "ready."** You will never feel ready. Apply while learning; interviews are diagnostic.
14. **Treating the model as deterministic.** Design for variance from day one.
15. **No error analysis.** Reading 50 real failures teaches more than any course. Almost nobody does it.
16. **Certification collecting.** Hiring managers do not care.
17. **Learning three cloud providers shallowly.** Pick one, go deep.
18. **Believing benchmark numbers.** Your eval set is the only benchmark that matters.
19. **Ignoring security.** Prompt injection is not theoretical; it's a data breach waiting for a trigger.
20. **Working alone forever.** Feedback compounds. Open-source, communities, code review.

---

## 27. Glossary

**Agent** — An LLM in a loop that takes actions via tools until a goal is met.
**Alignment** — Making model behavior match human intent and values.
**Attention** — Mechanism letting each token weigh the relevance of every other token.
**BM25** — Classic keyword-based retrieval ranking function.
**Chunking** — Splitting documents into pieces for embedding and retrieval.
**Context window** — Maximum tokens a model can process in one request.
**Cross-encoder** — Model that scores a query-document pair jointly; used for reranking. Slower, more accurate than bi-encoders.
**Distillation** — Training a small model to imitate a large one.
**DPO** — Direct Preference Optimization; simpler alternative to RLHF.
**Embedding** — Dense vector representing meaning.
**Few-shot** — Providing examples in the prompt.
**Fine-tuning** — Updating model weights on task-specific data.
**Grounding** — Tying generated claims to retrieved source material.
**Guardrails** — Input/output checks enforcing safety and policy.
**Hallucination** — Confidently generated false content.
**HNSW** — Graph-based approximate nearest neighbor index.
**Hybrid search** — Combining dense (vector) and sparse (keyword) retrieval.
**Inference** — Running a trained model to produce output.
**KV cache** — Cached attention keys/values enabling fast token-by-token generation.
**LLM-as-judge** — Using a model to grade another model's output.
**LoRA** — Low-Rank Adaptation; parameter-efficient fine-tuning.
**MCP** — Model Context Protocol; open standard for connecting models to tools and data.
**MoE** — Mixture of Experts; sparse architecture activating a subset of parameters.
**Multimodal** — Handling more than one input/output type (text, image, audio).
**Perplexity** — Measure of how surprised a model is by text; lower is better.
**PEFT** — Parameter-Efficient Fine-Tuning.
**Prefill** — Processing the input prompt before generation begins.
**Prompt caching** — Reusing computation for repeated prompt prefixes.
**Prompt injection** — Attack where untrusted input hijacks model instructions.
**Quantization** — Reducing numeric precision of weights to save memory/compute.
**RAG** — Retrieval-Augmented Generation.
**Reranking** — Re-scoring retrieved candidates with a more accurate model.
**RLHF** — Reinforcement Learning from Human Feedback.
**RoPE** — Rotary Position Embedding; how modern LLMs encode position.
**Scaling laws** — Empirical relationships between compute/data/parameters and performance.
**SFT** — Supervised Fine-Tuning.
**Speculative decoding** — Small model drafts tokens, large model verifies, for speed.
**Streaming** — Sending tokens as generated rather than waiting for completion.
**System prompt** — Instructions setting model behavior for a conversation.
**Temperature** — Sampling parameter controlling randomness.
**Token** — Sub-word unit; the atomic input/output of an LLM.
**Tool use / function calling** — Model requesting execution of a defined function.
**Top-p / nucleus sampling** — Sampling from the smallest token set covering probability mass p.
**Vector database** — Store optimized for similarity search over embeddings.
**vLLM** — High-throughput inference server using continuous batching and PagedAttention.
**Zero-shot** — Task performance with no examples provided.

---

## 28. Self-Assessment Checklist

Tick honestly. Untick anything you'd fail to explain in an interview.

### Foundations
- [ ] Build, test, containerize, and deploy a typed Python API service without help
- [ ] Write correct async code with proper concurrency limits
- [ ] Design a normalized schema and write efficient SQL with indexes
- [ ] Debug a production issue from logs and traces alone
- [ ] Explain gradient descent, cross-entropy, and cosine similarity clearly

### LLM fundamentals
- [ ] Explain tokenization and its practical consequences
- [ ] Explain attention, KV cache, and why long contexts are expensive
- [ ] Explain temperature vs top_p and when to change each
- [ ] Choose between models with a defensible cost/latency/capability argument
- [ ] Have built a small transformer from scratch, once

### Application engineering
- [ ] Write prompts that reliably produce schema-valid structured output
- [ ] Implement streaming, retries, backoff, timeouts, and fallbacks
- [ ] Implement prompt caching and measure the savings
- [ ] Version prompts and test them in CI
- [ ] Explain and implement model routing

### RAG
- [ ] Build a full ingestion → chunk → embed → index pipeline
- [ ] Implement hybrid search with rank fusion
- [ ] Implement reranking and measure its impact
- [ ] Enforce document-level permissions at retrieval time
- [ ] Diagnose whether a bad answer is a retrieval or generation failure

### Agents
- [ ] Build an agent loop from scratch with termination and cost caps
- [ ] Design tools with good descriptions and recoverable errors
- [ ] Trace and debug a multi-step agent run
- [ ] Explain when a workflow beats an agent

### Evaluation
- [ ] Build a golden eval set from real traffic
- [ ] Implement code-based and LLM-judge graders
- [ ] Validate a judge against human labels
- [ ] Report retrieval metrics (recall@k, NDCG) correctly
- [ ] Run evals in CI and block regressions

### Production
- [ ] Instrument full request tracing with cost and latency
- [ ] Set up alerting on quality, latency, and spend
- [ ] Perform a canary rollout and a rollback
- [ ] Cut a real system's cost by a measured percentage
- [ ] Load-test and report p95/p99 latency

### Security
- [ ] Explain direct vs indirect prompt injection and the lethal trifecta
- [ ] Implement input and output guardrails
- [ ] Red-team your own application and document findings
- [ ] Handle PII redaction before third-party API calls

### Career
- [ ] 3–5 polished, deployed, documented projects with metrics
- [ ] One-page resume with quantified impact
- [ ] Can present any project for 20 minutes under questioning
- [ ] Can design an AI system end to end on a whiteboard
- [ ] Published writing or open-source contributions

**Scoring:** under 40% — you're at the foundations stage, keep building. 40–70% — you're employable as a junior/mid AI engineer, start applying. Over 70% — you're competitive for senior roles; specialize and go deep.

---

## Final Word

The field is loud. Most of the noise is people describing prototypes as products and demos as breakthroughs.

The signal is simple and boring: **can you ship a reliable AI system that people use, measure whether it's actually good, and keep it running affordably?**

Everything in this document serves that one question. Most people never get there because they optimize for feeling informed rather than being capable. Don't be most people.

Build. Measure. Ship. Repeat.
