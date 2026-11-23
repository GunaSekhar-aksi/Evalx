Before the plan, one correction. You said "Date class." `java.util.Date` dates from 1996 and has real problems: you can change a Date after creating it, and its sibling `Calendar` numbers January as 0. If you write `new Date()` in an interview in 2026, expect a strange look. What you actually want is **`java.time`**, the package your own code already uses (`Instant`, `Duration`) without you really knowing why. We'll fix that.

---

## Phase 1: Time (`java.time`), about 4 days

**The one idea everything rests on:** Java has two kinds of time.
- **Machine time** is one number that keeps counting up: milliseconds since 1 Jan 1970. It's for timestamps, expiry and rate limits. → `Instant`, `System.currentTimeMillis()`
- **Human time** is a calendar date like "19 Nov 2026" with no clock attached. It's for ages, deadlines and "days between". → `LocalDate`, `LocalDateTime`

Most confusion with time comes from mixing these two up.

| Day | Topic | Methods you must know cold |
|---|---|---|
| 1 | Machine time | `System.currentTimeMillis()`, `System.nanoTime()` (for measuring how long code takes, never for telling the time), `Instant.now()`, `toEpochMilli()`, `Instant.ofEpochMilli()`, `plus/minus`, `isBefore/isAfter` |
| 2 | Human time | `LocalDate.now()`, `LocalDate.of(2026,11,19)`, `plusDays/minusMonths`, `getDayOfWeek()`, `isLeapYear()`, `withDayOfMonth()`, `LocalDateTime`, `LocalTime` |
| 3 | **Differences** (what you asked for) | `ChronoUnit.DAYS.between(a, b)`, `Period.between()` (gives years/months/days), `Duration.between()` (gives hours/minutes/seconds), `Duration.ofSeconds(1)`, `toMillis()` |
| 4 | Parsing, formatting, zones | `DateTimeFormatter.ofPattern("dd-MM-yyyy")`, `LocalDate.parse()`, `format()`, `ZoneId.of("Asia/Kolkata")`, `ZonedDateTime` |

**Exercises:** compute your age in days, find the day of the week your birthday falls on in 2030, list every Monday in a month, check whether a 30-minute link has expired, and time a loop with `nanoTime`.

**You're done when** you can explain without looking why `Period.between` and `ChronoUnit.DAYS.between` give different answers for the same two dates.

---

## Phase 2: Random, about 1.5 days

| Topic | What to know |
|---|---|
| `Random` | `nextInt()`, `nextInt(bound)` (0 up to bound−1), `nextInt(origin, bound)`, `nextDouble()`, `nextBoolean()`, `ints()` streams |
| **Seeds** | `new Random(42)` produces the same sequence every run. That's useful for tests. In a URL shortener it's a disaster. |
| `SecureRandom` | Hard to predict, so use it for tokens, codes and passwords. Slower. |
| `ThreadLocalRandom.current()` | Use this inside threads. A single `Random` shared by many threads makes them compete for it. |
| Others | `Math.random()`, `UUID.randomUUID()`, `Collections.shuffle()` |

**Exercises:** roll dice 10,000 times and count each face, generate a random 7-character base62 code by hand, shuffle a deck, and show that seed 42 gives the same output twice.

**You're done when** you can answer "why doesn't random generation alone guarantee unique codes?" The answer is that randomness never guarantees uniqueness. Only a check-and-insert done as **one step that can't be interrupted** does. That leads straight into Phase 3.

---

## Phase 3: Threads, about 5 days (this is the hard one)

| Day | Topic | Know |
|---|---|---|
| 1 | Basics | `Thread`, `Runnable`, lambdas, `start()` vs `run()` (a classic trap), `join()`, `sleep()`, `Thread.currentThread().getName()` |
| 2 | **Race conditions** | Break things on purpose: two threads each run `count++` 100,000 times on a shared counter, and the total comes out wrong. Understand why. |
| 3 | Fixes | `synchronized` (on a method or on an object), `volatile` (what it does and what it does **not** do), `AtomicInteger` / `AtomicLong` with `incrementAndGet()` |
| 4 | Concurrent collections | `ConcurrentHashMap`: `putIfAbsent`, `computeIfAbsent`, `merge`. Why "check, then put" is broken and `putIfAbsent` isn't. |
| 5 | Thread pools | `ExecutorService`, `Executors.newFixedThreadPool`, `submit`, `Future.get()`, `Callable`, `shutdown`, `CountDownLatch` (to make all threads start at the same moment) |

**You're done when** you can say in one sentence why `if (!map.containsKey(k)) map.put(k, v)` breaks when 10 threads run it, and what replaces it.

---

## Final exam

Rewrite the URL shortener and the rate limiter **from a blank file**, with no copy-paste. Then explain every timing and thread decision out loud, the way you would in an interview. If you can't, you're not done.

---

Total: about 10 days at 1 to 2 hours a day. Each lesson works the same way: a short explanation, then code you run yourself, then exercises with no answers until you've tried.

Say **"start Time day 1"** and we begin.
