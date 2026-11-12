# MSAPT — Online Assessment MCQs: Aptitude, Probability, C/C++ output (Microsoft)

> Weightage: OA filter only. Reported: ~25 MCQs in 30 min on C, C++, probability aptitude and OS
> (older on-campus format); newer OAs are mostly coding + a few fundamentals MCQs. Low depth, high speed.

## Probability: the patterns that keep repeating
1. **Basic:** P(event) = favorable / total. Two dice sum to 7: 6/36 = 1/6.
2. **Complement:** P(at least one) = 1 − P(none). At least one head in 3 tosses: 1 − 1/8 = 7/8.
3. **Conditional:** P(A|B) = P(A∩B)/P(B). A family has 2 kids and at least one is a boy. P(both boys) = 1/3, not 1/2 (the outcomes are BB, BG, GB).
4. **Bayes:** P(A|B) = P(B|A)·P(A)/P(B). The classic is the disease-test false-positive question; the answer is always lower than people expect.
5. **Expected value:** E = Σ x·p(x). Expected number of tosses to get the first head = 1/p = 2.
6. **Birthday problem:** 23 people gives >50% chance of a shared birthday.
7. **Combinations:** nCr = n!/(r!(n−r)!). Arrangements of a word with repeated letters = n!/(a!·b!...).

## Classic puzzles (sometimes asked live)
- **25 horses, 5 tracks, find the top 3:** 7 races (5 heats, 1 race of the winners, 1 final race among the remaining candidates).
- **Two eggs, 100 floors:** 14 drops minimum (drop from 14, then 27, then 39...; the gaps shrink by 1 each time).
- **Bridge and torch (1, 2, 5, 10 minutes):** 17 minutes. Send 1+2 over, 1 returns, 5+10 cross, 2 returns, 1+2 cross.
- **Find the heavier ball among 8 with a balance:** 2 weighings (split 3/3/2).
**How to answer live:** Think out loud. They're grading your reasoning, not whether you've memorized the answer.

## C/C++ output traps (the MCQs lean on these)
```c
int i = 5; printf("%d", i++ + ++i);   // UNDEFINED behavior: modified twice with no sequence point
```
- `sizeof` on an array parameter inside a function gives the pointer size, not the array size (arrays decay to pointers).
- `char *s = "hi"; s[0] = 'H';` is undefined behavior (string literals are read-only).
- Integer division truncates: `7/2 == 3`. Mixing signed and unsigned: `-1 > 1u` is TRUE, because -1 gets converted to a huge unsigned value.
- `static` local variables keep their value between calls; a `static` global is visible only in its file.
- A struct's size includes padding for alignment: `struct{char c; int i;}` is usually 8 bytes, not 5.
- A missing `break` in a switch falls through to the next case.
- `a[i] == *(a+i) == i[a]`. Yes, `i[a]` compiles.
- In C++, a virtual call inside a constructor resolves to the *base* class version.

## Quick OS/DBMS MCQ facts (see the full files)
- The banker's algorithm is for deadlock *avoidance*.
- Paging is implemented with hardware support (the MMU) managed by the OS.
- Belady's anomaly happens with FIFO page replacement, never with LRU or Optimal.
- A system call switches from user mode to kernel mode.
- A primary key can't be NULL; a foreign key can.

## Speed tactics
Skip anything that takes more than a minute and come back to it. Eliminate obviously wrong options first. If wrong answers carry negative marking, don't guess blindly.
