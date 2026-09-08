# L0.1 — Python Mastery Curriculum

**Target:** Section 3.1 of the AI Engineer roadmap.
**Starting point:** You can write loops, solve DSA problems, and use lists and dicts.
**Ending point:** You can write typed, tested, async, packaged Python that a senior engineer would approve in code review.
**Realistic duration:** 4 weeks at 3–4 hours/day. 6–8 weeks at 1.5 hours/day.

---

## Table of Contents

- [How To Use This](#how-to-use-this)
- [The Spine Project](#the-spine-project)
- [The Practice Method](#the-practice-method)
- [Week 1 — Core Language](#week-1--core-language)
- [Week 2 — Typing + Data Modeling](#week-2--typing--data-modeling)
- [Week 3 — Async](#week-3--async)
- [Week 4 — Testing, Packaging, Performance](#week-4--testing-packaging-performance)
- [Master Resource List](#master-resource-list)
- [Daily Routine](#daily-routine)
- [Graduation Test](#graduation-test)

---

## How To Use This

**Rules:**

1. **Never watch a tutorial without your editor open.** Pause, type it, break it, fix it. Watching Python is like watching gym videos.
2. **Every topic ends with a drill.** If you skip drills you will forget everything within a week. This is not motivational advice, it's how memory works.
3. **The spine project is mandatory.** Isolated exercises don't stick. You need one codebase you keep returning to and improving.
4. **When confused, read the official docs.** Python's documentation is genuinely excellent and most people never open it because a YouTube thumbnail was easier.
5. **Do not move to the next week until the checkpoint passes.** Speed here produces the exact half-knowledge that gets people rejected.

**A note on resources:** I'm listing more than you need. Pick ONE primary per topic. Reading five explanations of decorators is procrastination with extra steps.

---

## The Spine Project

You will build **one tool**, four times, each time better. Call it `fetchbench`.

**What it does:** takes a list of URLs (or items), processes each one, and reports results with timing and error handling.

Why this project: it is structurally identical to every LLM application you'll ever write. Many I/O-bound calls, failures to handle, results to validate, timings to measure. You're building a miniature LLM pipeline without the LLM.

| Week | Version | What it gains |
|---|---|---|
| 1 | `v1` | Works. Uses generators, decorators, context managers, custom exceptions. Sync. |
| 2 | `v2` | Fully typed, `mypy --strict` clean. Pydantic models for config and results. |
| 3 | `v3` | Async. Concurrent with a semaphore limit. Retries with backoff. Timeouts. |
| 4 | `v4` | Tested with pytest. Packaged with uv. Profiled. Logged properly. |

By week 4 you have a real repo. That repo is worth more than 200 LeetCode problems on your resume.

**Setup today:**
```bash
mkdir fetchbench && cd fetchbench
git init
# we'll do proper packaging in week 4; for now just write code
```

Commit at the end of every single day. No exceptions. Your commit history is a study log.

---

## The Practice Method

Most people "learn" by reading and feeling smart. Here's what actually plants things in your brain:

### The four-step cycle (per concept)

1. **Read/watch once** (20–40 min). No notes. Just understand the shape.
2. **Type the examples yourself** (20 min). Not copy-paste. Type them. Muscle memory is real.
3. **Break it deliberately** (15 min). Remove a line. Change an order. Predict the error *before* you run it. Were you right? If not, you didn't understand it.
4. **Drill from blank file** (30–60 min). Close everything. Write the thing from scratch. Struggle. Look up only syntax, never concepts.

### The explanation test

After each topic, write 5 sentences explaining it to someone who doesn't know Python. If you can't, you learned the syntax and not the idea. Do this in a `notes.md` in your repo.

### Spaced repetition

Every Monday, redo one drill from the previous week from a blank file. It'll be embarrassing the first time. That's the point — the forgetting is the signal that the practice worked.

### What to STOP doing

- Stop grinding DSA daily. Keep it to 2–3 problems/week to stay warm. It's not building the skill you need right now.
- Stop reading "Top 10 Python Tricks" articles. Zero retention, pure dopamine.
- Stop starting new tutorials before finishing the current one.

---

## Week 1 — Core Language

**Goal:** Stop writing Python like it's Java-with-fewer-semicolons. Learn what the language actually gives you.

### Day 1 — Data model & data types (the real ones)

**What you're actually learning:** Python objects are not what you think. Everything is an object with a type, identity, and value. Mutability rules explain 80% of beginner bugs.

**Concepts:**
- Mutable vs immutable (`list` vs `tuple`, `dict` vs `frozenset`)
- Reference semantics — why `a = b` doesn't copy
- The mutable default argument trap (`def f(x=[])` — a classic interview question)
- Shallow vs deep copy (`copy` module)
- `is` vs `==`, and `id()`
- Truthiness rules
- `dict` ordering guarantees, `dict.get`, `setdefault`, `defaultdict`
- Sets and set operations
- String immutability, f-strings (including `f"{x=}"` and format specs)
- Unpacking: `a, *rest = items`, `**merge`

**Resources (pick one primary):**
- Real Python: "Python's Mutable vs Immutable Types"
- Python docs → Tutorial → sections 3, 5 (Data Structures)
- Book: *Fluent Python* (Ramalho), Ch. 2–3 — the best book on this material, period

**Drill:**
```
1. Write a function that takes a nested dict and returns a deep copy WITHOUT using
   copy.deepcopy. Recursion. Handle dicts, lists, and scalars.
2. Write 10 short snippets that demonstrate a mutability bug, then fix each one.
3. Predict the output of 15 `is` vs `==` comparisons before running them.
```

**Explanation test:** Why does modifying a list inside a function change it outside, but modifying an int doesn't?

---

### Day 2 — Comprehensions & iteration protocol

**What you're actually learning:** Python has a protocol for "things you can loop over." Once you see it, generators and `itertools` stop being magic.

**Concepts:**
- List / dict / set comprehensions, nested, with conditionals
- Generator expressions and when they beat comprehensions (memory)
- The iterator protocol: `__iter__`, `__next__`, `StopIteration`
- `iter()`, `next()` with default
- `enumerate`, `zip`, `zip(strict=True)`, `reversed`, `sorted(key=...)`
- `any`, `all`, `sum`, `min`/`max` with `key`
- When a comprehension becomes unreadable and should be a loop (this is a real judgment skill)

**Resources:**
- Real Python: "When to Use a List Comprehension in Python"
- Talk: Raymond Hettinger — "Transforming Code into Beautiful, Idiomatic Python" (YouTube). Watch the whole thing. Twice.
- *Fluent Python* Ch. 17

**Drill:**
```
1. Rewrite 15 of your old DSA solutions using comprehensions where it IMPROVES
   readability. Note the 3 cases where it made it worse. That judgment is the lesson.
2. Implement your own `enumerate`, `zip`, and `range` as classes with __iter__/__next__.
3. Build a custom iterator class that reads a file in fixed-size chunks.
```

---

### Day 3 — Generators (high value, badly understood)

**What you're actually learning:** Functions that pause. This is the foundation for lazy processing, streaming, and — directly relevant — handling streamed LLM responses token by token.

**Concepts:**
- `yield`, generator functions vs generator objects
- Lazy evaluation — why you can generate infinite sequences
- `yield from` and delegation
- Generator pipelines (chaining generators for data processing)
- `send()`, `throw()`, `close()` — know they exist, understand `send()` conceptually
- Memory profile: generator vs list on a large dataset
- `itertools`: `chain`, `islice`, `groupby`, `count`, `cycle`, `takewhile`, `batched` (3.12+)

**Resources:**
- **David Beazley — "Generators: The Final Frontier"** or "Generator Tricks for Systems Programmers" (YouTube + his slides at dabeaz.com). This is the definitive treatment. Dense. Worth it.
- Real Python: "How to Use Generators and yield in Python"
- Python docs → `itertools` module (read the whole page and the recipes section)

**Drill:**
```
1. Write a generator pipeline that reads a large log file, filters lines, parses them,
   and yields dicts. Never load the file into memory.
2. Write an infinite fibonacci generator. Use islice to take the first 100.
3. Implement `batched(iterable, n)` yourself before looking at the stdlib version.
4. Compare memory usage: list of 10M ints vs generator. Use sys.getsizeof and
   tracemalloc. Actually measure it.
```

**Explanation test:** Why does a generator use constant memory when a list doesn't?

---

### Day 4 — Functions deep: closures, `*args/**kwargs`, first-class functions

**What you're actually learning:** Functions are objects. Once that lands, decorators become obvious instead of terrifying.

**Concepts:**
- Functions as objects: assign, pass, return, store in dicts
- `*args`, `**kwargs` — packing and unpacking, both in definitions and calls
- Positional-only (`/`) and keyword-only (`*`) parameters
- Default arguments and their evaluation time (the mutable default trap, again — it matters)
- Closures: functions that capture enclosing scope
- `nonlocal` vs `global`
- LEGB scope resolution
- `lambda` and when it's appropriate (rarely)
- `functools.partial`

**Resources:**
- Real Python: "Python Inner Functions" and "Defining Your Own Python Function"
- Corey Schafer YouTube: "Python Tutorial: Closures"
- *Fluent Python* Ch. 7

**Drill:**
```
1. Write a counter factory using a closure. Then rewrite it as a class. Compare.
2. Write a function `make_multiplier(n)` returning a function. Then make a list of
   10 multipliers in a loop — hit the classic late-binding bug, then fix it.
3. Write a function that accepts ANY signature and logs exactly what it received.
4. Build a tiny plugin registry: a dict mapping names to functions, with a
   register function.
```

---

### Day 5 — Decorators

**What you're actually learning:** Wrapping behavior around functions. You will use these constantly — for retries, timing, caching, and logging around LLM calls.

**Concepts:**
- The `@` syntax as sugar for `f = decorator(f)`
- Writing a decorator that preserves signature (`functools.wraps` — never skip it)
- Decorators with arguments (three levels of nesting; this is where people quit)
- Stacking decorators and evaluation order
- Class-based decorators
- Built-in decorators: `@property`, `@staticmethod`, `@classmethod`, `@functools.cache`, `@functools.lru_cache`, `@dataclass`
- Real-world decorators: timing, retry with backoff, logging, rate limiting, validation

**Resources:**
- Real Python: "Primer on Python Decorators" — the best single resource on this topic
- ArjanCodes YouTube — decorator videos
- Python docs → `functools`

**Drill:**
```
1. Write @timer that prints execution time.
2. Write @retry(times=3, delay=1) with exponential backoff. THIS ONE MATTERS —
   you will write this exact decorator in every AI project you ever build.
3. Write @cache yourself (dict keyed on args) before using functools.cache.
4. Write @validate_types that checks arguments against annotations at runtime.
5. Stack all of them on one function. Predict the execution order before running.
```

**Explanation test:** What does `functools.wraps` fix, and how would you notice it's missing?

---

### Day 6 — Context managers & exceptions

**What you're actually learning:** Guaranteed cleanup, and how to fail properly. Sloppy exception handling is the #1 marker of an amateur codebase.

**Concepts — context managers:**
- `with` statement, why it exists
- `__enter__` / `__exit__` protocol
- `contextlib.contextmanager` decorator (generator-based, much easier)
- `contextlib.suppress`, `ExitStack`, `closing`
- Multiple context managers in one `with`
- Real uses: files, DB connections, locks, timers, temporary state changes

**Concepts — exceptions:**
- The exception hierarchy (`BaseException` → `Exception` → everything)
- `try/except/else/finally` — and what `else` is actually for
- Catching specific exceptions. **Never bare `except:`.** Rarely bare `except Exception:`.
- Custom exception classes, exception hierarchies for your own library
- `raise ... from ...` (exception chaining) — preserves the cause
- Re-raising correctly
- EAFP vs LBYL (Python prefers "ask forgiveness")
- `ExceptionGroup` and `except*` (3.11+) — relevant for async
- What NOT to do: swallowing exceptions, using exceptions for control flow, catching too broadly

**Resources:**
- Real Python: "Context Managers and Python's with Statement"
- Real Python: "Python Exceptions: An Introduction"
- Python docs → `contextlib`

**Drill:**
```
1. Write a context manager class that times a code block.
2. Rewrite it with @contextmanager. Compare line counts.
3. Write a context manager that temporarily changes a dict and restores it on exit —
   including when an exception is raised.
4. Design an exception hierarchy for a fake API client:
   ClientError → (AuthError, RateLimitError, TimeoutError, ServerError).
   Write code that handles each differently.
5. Write a function that catches an exception, adds context, and re-raises with `from`.
```

---

### Day 7 — Classes & the object model (light) + BUILD v1

**Concepts:**
- Classes, `__init__`, instance vs class attributes
- Dunder methods: `__repr__` (always write this), `__str__`, `__eq__`, `__hash__`, `__len__`, `__contains__`, `__call__`
- `@property` for computed attributes
- `@dataclass` — and why you should reach for it before writing `__init__`
- `__slots__` (know it exists)
- Composition over inheritance
- ABCs and `Protocol` (preview of week 2)

**Resources:**
- Real Python: "Python Classes: The Power of Object-Oriented Programming"
- Real Python: "Data Classes in Python 3.7+"
- *Fluent Python* Ch. 1 (the Python Data Model — genuinely one of the best chapters in any programming book)

**BUILD `fetchbench` v1:**
```
Requirements:
- Reads a list of items from a JSON file
- Processes each with a function that sometimes fails (simulate with random)
- Uses a @retry decorator with backoff
- Uses a @timer decorator
- Uses a context manager for the overall run timing
- Uses a generator to stream results rather than building a list
- Custom exception hierarchy for failures
- @dataclass for the result objects
- Prints a summary: successes, failures, total time, per-item times
```

### Week 1 checkpoint

- [ ] I can write a decorator with arguments from a blank file
- [ ] I can explain why generators use constant memory
- [ ] I can write a context manager two different ways
- [ ] I have never written a bare `except:` this week
- [ ] `fetchbench` v1 runs and I understand every line
- [ ] I can explain the mutable default argument trap

---

## Week 2 — Typing + Data Modeling

**Goal:** Make the computer catch your bugs before runtime. Non-negotiable for production code, and the whole modern AI stack is built on this.

### Day 8 — Type hints fundamentals

**Concepts:**
- Why types: catching bugs, IDE autocomplete, self-documenting code, safe refactoring
- Basic annotations: variables, parameters, return types
- Built-in generics: `list[int]`, `dict[str, int]`, `tuple[int, str]`, `set[str]` (modern syntax, not `typing.List`)
- `Optional[X]` / `X | None` — and why "this can be None" is the most valuable annotation you'll write
- `Union` / `X | Y`
- `Any` and why it's an escape hatch, not a solution
- `Callable[[int, str], bool]`
- Type aliases
- `Literal` for constrained strings
- `Final`, `ClassVar`
- Annotating `*args` and `**kwargs`
- Forward references and `from __future__ import annotations`

**Resources:**
- **mypy documentation → "Type hints cheat sheet"** — the single most useful typing page on the internet. Bookmark it.
- Real Python: "Python Type Checking (Guide)"
- Python docs → `typing` module

**Drill:**
```
1. Take fetchbench v1 and annotate every function. Every one.
2. Run `mypy .` — fix every error. Then run `mypy --strict .` and fix those too.
   This will hurt. That's the education.
3. Annotate 10 of your old DSA solutions.
```

---

### Day 9 — Advanced typing

**Concepts:**
- `TypeVar` and generic functions
- Generic classes (`class Stack[T]:` in 3.12+, or `Generic[T]` before)
- `Protocol` — structural typing / duck typing with type safety. **Important.** This is how you type "anything with a `.read()` method" without inheritance.
- `TypedDict` for dict shapes
- `NewType`
- `overload` for functions with multiple signatures
- `Self` type
- `Annotated` (used heavily by Pydantic and FastAPI)
- Narrowing: `isinstance` checks, `assert`, `TypeGuard`
- Variance (covariance/contravariance) — understand the concept, don't memorize the rules
- `# type: ignore` and when it's acceptable (rarely, with a comment explaining why)

**Resources:**
- mypy docs → "Generics", "Protocols and structural subtyping"
- Real Python: "Python Protocols: Leveraging Structural Subtyping"

**Drill:**
```
1. Write a generic `Result[T]` class that holds either a value or an error.
2. Define a Protocol for "something that can be processed" and write a function
   that accepts anything satisfying it.
3. Write a TypedDict for a config file shape and load JSON into it.
4. Write an overloaded function: takes str → returns str, takes list → returns list.
```

---

### Day 10 — Tooling: mypy / pyright / ruff

**Concepts:**
- Installing and configuring mypy (`pyproject.toml` config)
- `--strict` mode and what each flag does
- pyright / basedpyright (faster, used by VS Code's Pylance)
- **ruff** — linter and formatter, replaces flake8 + black + isort. Extremely fast, now the default.
- Pre-commit hooks
- Running type checks in CI

**Resources:**
- mypy docs → "Configuring mypy"
- Ruff documentation (astral.sh/ruff)

**Drill:**
```
1. Add a pyproject.toml with mypy strict config and ruff config to fetchbench.
2. Set up pre-commit so you cannot commit unformatted or untyped code.
3. Deliberately introduce 5 type bugs. Confirm mypy catches all 5.
```

---

### Days 11–13 — Pydantic v2

**What you're actually learning:** Runtime validation. Type hints alone are checked by mypy but ignored at runtime. Pydantic *enforces* them. This is what every AI framework uses for structured LLM output — the exact mechanism by which you make a model return valid JSON.

**Concepts:**
- `BaseModel`: definition, instantiation, validation
- Field types and coercion rules (what Pydantic will and won't convert)
- `Field()`: defaults, constraints (`gt`, `max_length`, `pattern`), aliases, descriptions
- `ValidationError` — reading it properly, `.errors()`
- Nested models
- Optional fields, default factories
- `field_validator` and `model_validator` (before/after modes)
- `computed_field`
- `model_config` / `ConfigDict`: `strict`, `frozen`, `extra="forbid"`
- Serialization: `model_dump()`, `model_dump_json()`, `model_validate()`, `model_validate_json()`
- **`model_json_schema()`** — generates JSON Schema. This is literally how LLM tool-calling and structured output work under the hood.
- Custom types with `Annotated`
- `pydantic-settings` for environment-based config
- Pydantic v1 vs v2 differences (you'll hit v1 code in the wild; know that `.dict()` became `.model_dump()`)
- Performance: it's Rust-backed and fast, but validation isn't free

**Resources:**
- **Pydantic official docs** (docs.pydantic.dev) — genuinely excellent, read Concepts → Models, Fields, Validators, Serialization
- Pydantic docs → "Migration guide" for v1→v2
- ArjanCodes YouTube — Pydantic videos

**Drill:**
```
1. Model a realistic API response with nested objects and optional fields.
   Feed it good JSON, then 10 varieties of malformed JSON. Read every error.
2. Write a validator that normalizes a phone number field.
3. Write a model_validator that checks a cross-field constraint
   (e.g. end_date must be after start_date).
4. Generate the JSON schema for a model. Read it. This is what you'd send to an LLM
   as a tool definition — understanding this now saves you confusion later.
5. Build a Settings class with pydantic-settings that reads from env vars with
   defaults and validation.
```

---

### Day 14 — BUILD v2

**Upgrade `fetchbench`:**
```
- Every function fully annotated, `mypy --strict` clean, zero ignores
- ruff formatted and linted, zero warnings
- Config loaded and validated via a Pydantic Settings model
- Input items parsed into Pydantic models with validation
- Results as Pydantic models, serializable to JSON
- Custom validators on at least two fields
- A Protocol defining the "processor" interface
- Generic Result[T] type for success/failure
- Commit it
```

### Week 2 checkpoint

- [ ] `mypy --strict` passes on my project with zero `type: ignore`
- [ ] I can explain the difference between mypy checking and Pydantic validating
- [ ] I can write a Protocol and explain why it beats an ABC here
- [ ] I can generate a JSON schema from a model and read it
- [ ] I understand what `Annotated` is for

---

## Week 3 — Async

**Goal:** The highest-value week for AI engineering. Every LLM call is I/O-bound. Sequential API calls are the most common and most expensive junior mistake in this field.

**Warning:** async is genuinely hard and everyone finds it confusing at first. Budget the full week. Do not skim it.

### Day 15 — Concurrency concepts (theory before syntax)

**Concepts:**
- Concurrency vs parallelism — precisely. Concurrency is dealing with many things at once; parallelism is doing many things at once.
- CPU-bound vs I/O-bound work
- The GIL: what it actually does, what it doesn't, and why it doesn't matter for I/O
- Three models in Python: threading, multiprocessing, asyncio — and when each is correct
- **Decision rule:** I/O-bound + many tasks → asyncio. CPU-bound → multiprocessing. Blocking library you can't change → threads.
- The event loop: single thread, cooperative multitasking, tasks yielding at `await` points
- Why one blocking call poisons the entire event loop

**Resources:**
- **Real Python: "Async IO in Python: A Complete Walkthrough"** — start here, it's the best on-ramp
- Talk: "Concurrency for People Who Hate Concurrency" or Raymond Hettinger's concurrency talks
- Python docs → `asyncio` → "High-level APIs"

**Drill:**
```
Write four versions of the same task (sleep 1s, 20 times):
  a) sequential
  b) threading
  c) multiprocessing
  d) asyncio
Time each. Explain the results. Then repeat with a CPU-bound task
(computing primes) and explain why the results INVERT.
This single exercise teaches more than any article.
```

---

### Day 16 — asyncio basics

**Concepts:**
- `async def` — coroutine functions vs coroutine objects
- `await` — what it actually does (yields control back to the loop)
- **Coroutines don't run until awaited or scheduled.** Common bug source.
- `asyncio.run()` — the entry point
- `asyncio.sleep()` vs `time.sleep()` — the second one blocks the whole loop
- Tasks: `asyncio.create_task()`, why tasks run concurrently but bare coroutines don't
- `asyncio.gather()` — run many, collect results, `return_exceptions=True`
- `asyncio.TaskGroup` (3.11+) — the modern preferred approach, with proper cancellation
- `asyncio.as_completed()` — process results as they arrive
- `asyncio.wait_for()` — timeouts
- `asyncio.timeout()` context manager (3.11+)

**Resources:**
- Real Python async walkthrough (continued)
- Python docs → asyncio → Tasks and Coroutines
- Łukasz Langa's asyncio videos on YouTube (long-form, excellent)

**Drill:**
```
1. Write a script that fetches 50 URLs sequentially. Time it. (use httpx)
2. Rewrite with gather. Time it. Understand the difference viscerally.
3. Rewrite with TaskGroup.
4. Add a per-request timeout.
5. Deliberately put a time.sleep(2) inside one coroutine. Observe everything freeze.
   THIS is the lesson — the whole loop stops. Now fix it with asyncio.sleep.
```

---

### Day 17 — Concurrency control & error handling

**Concepts:**
- `asyncio.Semaphore` — limiting concurrent operations. **Essential.** Without it you'll hit rate limits and get banned from every API you touch.
- `asyncio.Lock`, `Event`, `Queue` — coordination primitives
- Producer/consumer with `asyncio.Queue`
- Cancellation: `task.cancel()`, `CancelledError`, and why you must never swallow it
- `ExceptionGroup` and `except*` with TaskGroup
- `gather(return_exceptions=True)` vs TaskGroup's fail-fast — different semantics, pick deliberately
- Timeouts at multiple levels
- Graceful shutdown
- Retry logic in async code (async version of your week-1 decorator)

**Resources:**
- Python docs → asyncio → Synchronization Primitives, Queues
- Real Python: "Python's asyncio: A Hands-On Walkthrough"
- The `tenacity` library docs (production-grade retries, async-compatible)

**Drill:**
```
1. Fetch 200 URLs with max 10 concurrent. Semaphore. Verify the limit holds
   by logging active count.
2. Build a producer/consumer: one coroutine generates work, 5 workers consume.
3. Write an async @retry decorator with exponential backoff and jitter.
4. Make one task fail. Handle it three ways: gather default, gather with
   return_exceptions, TaskGroup. Explain the difference in behavior.
5. Implement graceful shutdown on Ctrl+C that finishes in-flight work.
```

---

### Day 18 — Async in practice

**Concepts:**
- `httpx` — async HTTP client (the modern `requests`). Connection pooling, `AsyncClient` reuse.
- **Reuse your client.** Creating a new client per request destroys performance. Classic mistake.
- `aiofiles` for async file I/O
- Async context managers: `async with`, `__aenter__`/`__aexit__`, `@asynccontextmanager`
- Async iterators and generators: `async for`, `async def` with `yield`
- **Streaming responses** — this is exactly how you'll consume LLM token streams
- `asyncio.to_thread()` — running blocking code without freezing the loop
- Mixing sync and async: the bridge patterns and their pitfalls
- Debugging async: `asyncio.run(..., debug=True)`, common "coroutine was never awaited" warnings

**Resources:**
- httpx documentation → Async support
- Python docs → `contextlib` → `asynccontextmanager`

**Drill:**
```
1. Write an async context manager that manages a shared httpx client.
2. Write an async generator that yields results as they complete.
3. Stream a large HTTP response chunk by chunk with `async for`.
   (This is token streaming with a different payload.)
4. Wrap a blocking library call with asyncio.to_thread and prove the loop
   stays responsive.
```

---

### Days 19–21 — BUILD v3 + consolidation

**Upgrade `fetchbench`:**
```
- Fully async processing
- httpx AsyncClient, created once, reused
- Semaphore limiting concurrency (configurable via Pydantic settings)
- Async retry decorator with exponential backoff and jitter
- Per-request timeout AND overall timeout
- TaskGroup for structured concurrency
- Results streamed via async generator as they complete
- Proper cancellation handling
- Benchmark output: sequential vs concurrent, with the speedup factor printed
```

Spend day 21 on consolidation: redo the week-1 and week-2 drills from a blank file. You'll have forgotten things. Good.

### Week 3 checkpoint

- [ ] I can explain the GIL correctly, including what it doesn't affect
- [ ] I can explain why `time.sleep` in a coroutine is a bug
- [ ] I can write a semaphore-limited concurrent fetcher from a blank file
- [ ] I know the difference between gather and TaskGroup error semantics
- [ ] I've measured a real speedup and can state the number

---

## Week 4 — Testing, Packaging, Performance

### Days 22–24 — pytest

**Concepts:**
- Test structure, naming conventions, `assert` (plain assert, pytest rewrites it)
- Running tests: `-v`, `-k`, `-x`, `--lf`, markers
- **Fixtures** — the core concept. Setup/teardown, scopes (function/module/session), `yield` fixtures, `conftest.py`, fixture composition
- `@pytest.mark.parametrize` — one test, many cases. Use it constantly.
- `pytest.raises` for expected exceptions
- `pytest.approx` for floats
- **Mocking**: `unittest.mock`, `MagicMock`, `patch`, `monkeypatch` fixture
- **Where to patch** — patch where it's *used*, not where it's defined. The single most common mocking mistake.
- `pytest-asyncio` for async tests
- Mocking HTTP: `respx` (for httpx) or `responses`
- Coverage: `pytest-cov`, and why 100% coverage is a vanity metric
- What to test: behavior not implementation; edge cases; error paths
- Test doubles: mock vs stub vs fake
- **Testing LLM code:** mock the API layer entirely, assert on the request you built and the parsing of a canned response. Never call a real API in a unit test.

**Resources:**
- **pytest official documentation** — start with "Get Started" and "How-to guides"
- Book: *Python Testing with pytest* (Brian Okken) — the standard reference
- Real Python: "Effective Python Testing With pytest"
- Podcast: *Test & Code* by Brian Okken

**Drill:**
```
1. Write tests for every function in fetchbench. Aim for meaningful coverage,
   not a number.
2. Parametrize a validation test with 15 cases including edge cases.
3. Mock httpx entirely with respx. Test success, 429, 500, and timeout paths.
4. Write a fixture that provides a configured test client, session-scoped.
5. Test the retry decorator: assert it retried exactly 3 times. (Hint: mock + call count.)
6. Write an async test with pytest-asyncio.
```

---

### Day 25 — Packaging with uv

**Concepts:**
- Virtual environments: what they are, why they exist
- **uv** — the modern tool (`uv init`, `uv add`, `uv sync`, `uv run`, `uv lock`)
- `pyproject.toml` structure: `[project]`, dependencies, optional-dependencies, `[tool.*]` config
- Lock files and reproducible builds
- Dependency resolution and version constraints (`>=`, `~=`, `==`)
- Dev dependencies vs runtime dependencies
- Building and structure: `src/` layout vs flat layout (use `src/`)
- Entry points / console scripts
- Semantic versioning
- Publishing to PyPI (know the process even if you don't publish)
- Alternatives you'll encounter: poetry, pip + venv + requirements.txt, pipx

**Resources:**
- **uv documentation** (docs.astral.sh/uv) — read Getting Started and Projects
- Python Packaging User Guide → "Packaging Python Projects"

**Drill:**
```
1. Restructure fetchbench into a proper src/ layout package with pyproject.toml.
2. Make it installable: `uv sync` then `uv run fetchbench items.json` works.
3. Add a console script entry point.
4. Separate dev dependencies (pytest, mypy, ruff) from runtime.
5. Delete your venv and rebuild from the lock file. Confirm it works identically.
```

---

### Day 26 — Standard library fluency

Read the docs. Actually read them. One page each, with the examples typed out.

| Module | What to know |
|---|---|
| `json` | `load`/`loads`/`dump`/`dumps`, custom encoders, `default=`, `indent` |
| `re` | compile, match vs search vs findall vs finditer, groups, named groups, sub, flags. Also: know when regex is the wrong tool. |
| `pathlib` | `Path`, `/` operator, `read_text`, `glob`, `mkdir(parents=True)`. Stop using `os.path`. |
| `itertools` | Read the whole page including recipes |
| `functools` | `wraps`, `cache`, `lru_cache`, `partial`, `reduce`, `singledispatch` |
| `dataclasses` | `field`, `default_factory`, `frozen`, `asdict`, `__post_init__` |
| `collections` | `defaultdict`, `Counter`, `deque`, `namedtuple`, `ChainMap` |
| `logging` | Loggers, handlers, formatters, levels, `logging.config`, why `print()` isn't logging, structured logging |
| `datetime` | Timezone-aware vs naive (always use aware), `timedelta`, ISO formatting |
| `enum` | `Enum`, `StrEnum`, `auto()` |
| `os` / `sys` | `environ`, `argv`, `exit` |
| `typing` | Already covered |
| `abc` | ABCs and abstract methods |
| `contextlib` | Already covered |
| `secrets` / `hashlib` / `uuid` | tokens, hashing, IDs |

**Logging deserves special attention.** Do this properly:
```
1. Set up a logger with a formatter including timestamp, level, module, and message.
2. Log to console at INFO and to a file at DEBUG simultaneously.
3. Add structured context (request IDs) to log records.
4. Replace every print() in fetchbench with proper logging.
```

**Resources:**
- Python docs → Library Reference (yes, the actual docs)
- Python docs → HOWTOs → "Logging HOWTO" and "Logging Cookbook"

---

### Day 27 — Performance & profiling

**Concepts:**
- Measure before optimizing. Always. Intuition about performance is reliably wrong.
- `timeit` for micro-benchmarks
- `cProfile` + `pstats` for function-level profiling
- `snakeviz` for visualizing profile output
- `line_profiler` for line-level detail
- `memory_profiler` / `tracemalloc` for memory
- Common Python performance traps: string concatenation in loops, repeated list lookups, unnecessary copies, calling functions in tight loops
- Algorithmic complexity beats micro-optimization (you know this from DSA — apply it)
- Caching: `functools.cache`, and manual memoization
- **numpy**: vectorization, why array operations beat loops, broadcasting, when the conversion overhead isn't worth it
- When to reach for a different tool entirely (C extension, Rust, different architecture)

**Resources:**
- Python docs → `profile` module
- Real Python: "Python Timer Functions" and profiling guides
- numpy docs → "NumPy: the absolute basics for beginners"

**Drill:**
```
1. Profile fetchbench. Find the actual bottleneck. Was it where you guessed?
   (It won't be.)
2. Write the same computation three ways: pure Python loop, comprehension,
   numpy vectorized. Time all three on 1M elements.
3. Find a slow function in your old DSA solutions. Profile it, optimize it,
   measure the improvement. Report the number.
4. Use tracemalloc to find memory growth in a loop.
```

---

### Day 28 — BUILD v4 + finish

**Final `fetchbench`:**
```
- src/ layout, uv-managed, installable, with console entry point
- pyproject.toml with mypy strict + ruff config
- Full test suite: unit tests, async tests, mocked HTTP, parametrized cases
- CI-ready (add a GitHub Actions workflow running ruff + mypy + pytest)
- Proper logging throughout, configurable level
- Profiled, with a documented performance note in the README
- README with: what it does, architecture, how to run, benchmark results
- Clean commit history
```

Then write the README properly. Include the sequential-vs-concurrent benchmark numbers. That table is the whole point.

---

## Master Resource List

### Books (pick 1–2, don't buy five)

| Book | Use it for |
|---|---|
| **Fluent Python** (Ramalho, 2nd ed) | The single best book for exactly your gap — "I know Python syntax but not Python." Read Ch. 1–3, 5, 7–9, 17. |
| **Python Testing with pytest** (Okken) | Week 4. The pytest reference. |
| Effective Python (Slatkin) | 125 short specific improvements. Excellent supplement, easy to read in gaps. |
| Python Cookbook (Beazley) | Reference for "how do I do X idiomatically" |

### YouTube (highest signal)

| Channel/Talk | Why |
|---|---|
| **Raymond Hettinger** — "Transforming Code into Beautiful, Idiomatic Python" | Core Python philosophy. Watch first. |
| **David Beazley** — generator and asyncio talks | Deepest treatment of generators/concurrency anywhere |
| **mCoding** (James Murphy) | Short, precise, correct. Excellent on typing and dunder methods. |
| **ArjanCodes** | Design patterns, clean code, Pydantic, dataclasses |
| **Corey Schafer** | Clear fundamentals, good for closures/decorators/OOP |

### Documentation (free, better than most courses)

- Python official docs — Tutorial, Library Reference, HOWTOs
- Real Python (realpython.com) — best written tutorials; many free
- mypy docs — typing cheat sheet
- Pydantic docs
- pytest docs
- uv docs (astral.sh)
- httpx docs

### Practice platforms

- **Exercism (Python track)** — has actual human mentors reviewing your code. This is the best practice site for *idiomatic* Python, which is exactly your gap. Not LeetCode. Exercism.
- Advent of Code past years — good for applying generators/itertools
- CodeWars — quick idiom drills

---

## Daily Routine

```
  0:00–0:10   Review yesterday's notes.md. Recall before reading.
  0:10–0:50   New material (video/reading), editor open, typing along
  0:50–1:00   Break. Actually break.
  1:00–2:00   Drills from blank file
  2:00–2:45   Spine project work
  2:45–3:00   Write notes.md entry (5-sentence explanation test) + commit
```

**Monday extra:** redo one drill from last week, cold.

**If you only have 1.5 hours:** cut the new material to 30 min, drills to 40, project to 20. Never cut the drills. Never skip the commit.

---

## Graduation Test

You've completed L0.1 when you can do all of this from a blank file, in under 90 minutes, no tutorials open:

> Write a CLI tool that reads a JSON config file, validates it with Pydantic, fetches N URLs concurrently with a configurable concurrency limit and per-request timeout, retries failures with exponential backoff, streams results as they complete, logs progress at INFO and details at DEBUG, handles Ctrl+C gracefully, is fully type-annotated and passes `mypy --strict`, and comes with pytest tests that mock the HTTP layer and cover the retry logic.

If you can do that, you have the Python foundation to be an AI engineer. Everything in Layers 6–9 becomes a matter of learning APIs rather than learning to program.

If you can't, you know exactly which parts to redo. That's what the checkpoints are for.

---

## Final Note

You said you know "loops, DSA, problem solving." That's real, and it means you can think algorithmically. But DSA is a puzzle skill and software engineering is a *systems* skill. The four weeks above are the bridge.

The hardest part isn't the material. It's that decorators and async will feel impenetrable for about three days each, and most people quit inside those three days and conclude they're "not a real programmer." You're not stuck; you're in the normal part where it hasn't clicked yet. Keep typing. It clicks.

Commit every day. In four weeks you'll look at the log and see what consistency produces.
