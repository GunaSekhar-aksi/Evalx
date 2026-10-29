You asked for 1000 questions. There aren't 1000. I found about 60 distinct problems, and every blog and prep site recycles the same ones. That's good news: the pool is small enough to finish. Nothing here is official Infosys material. It's all candidate reports and prep-site compilations, so treat it as probable, not guaranteed.

## What the "offline round" actually is

- One 2026 report describes an offline coding round at the assessment center with an easy array problem, a medium direction-matrix problem, and a medium LIS problem.
- Another reports an in-person round of 4 DSA questions from DP, graph and sliding window.
- A third reports the round on Infosys's Wingspan portal: 2 medium questions (cycles/connected components, and sorting + binary search) in 45 minutes. In the interview they were asked to write the same code on paper and explain it.
- Another candidate got a pick-one-of-four format with 45 minutes.

Question count and time vary by city and batch, so prep for the worst case. Also practice writing code on paper, not just in an IDE.

## A. Problems on PrepInsta's SP/DSE pages

PrepInsta's SP and DSE pages list these as pattern-based questions, with an easy/medium/hard ramp. The Codeforces matches are my guess from the constraints, so verify them before relying on them.

| # | Problem (my paraphrase) | Type | Practice link |
|---|---|---|---|
| 1 | Max monsters defeatable, each needs min XP and gives bonus XP | Greedy | none |
| 2 | Count arrays of length K (values ≤N) where each element divides the next, mod 10000 | DP | none |
| 3 | LIS where each adjacent pair satisfies (a&b)×2 < (a\|b) | DP + bits | [LIS](https://leetcode.com/problems/longest-increasing-subsequence/) |
| 4 | Minimize a binary string's value using swaps/flips with a cash limit | Greedy | none |
| 5 | Max XOR picking at most N/2 elements (N≤120) | XOR basis | [Max XOR of two numbers](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) (related) |
| 6 | Pick x in [0,K] maximizing the sum of (x XOR A[i]) | Bit greedy | none |
| 7 | Smallest base where M's digits are all the same | Math | none |
| 8 | Andy's vacation: longest free stretch after cancelling ≤K obligations | Sliding window | [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/) (similar) |
| 9 | Min days to make an array strictly decreasing, where day D digs 2^(D−1) | Greedy/bits | none |
| 10 | Min changes to turn an array into a "mountain" | Counting | none |
| 11 | Max equal pieces after rearranging a string | Math | none |
| 12 | Min exercises (each at most twice) to drain energy to ≤0 | Greedy | none |
| 13 | Heroes vs villains: min villains to remove from the front | Binary search | none |
| 14 | Split into K segments, cost = sum of (last − first occurrence) per value | DP optimization | [CF 1527E](https://codeforces.com/problemset/problem/1527/E) |
| 15 | Pack gifts into K boxes maximizing total distinct types | DP optimization | [CF 833B](https://codeforces.com/problemset/problem/833/B) |
| 16 | Zero an array: range −1 costs X, single reset costs Y | DP/greedy | none |
| 17 | Min inversions after XORing the whole array with B | Trie/divide | [CF 1416C](https://codeforces.com/problemset/problem/1416/C) |

A GitHub prep sheet also names these as frequently reported 2025 questions: Minimum Platforms, Minimize Ugliness of Binary String, Wine Bottle Transport, Largest Number from Array, Minimum Coins, Remove Digit to Maximize Number, Difference Between Odd/Even Index Elements, Count Divisible Subarrays.

## B. Reported in onsite/offline rounds and interviews

| Problem | Source |
|---|---|
| [Gas Station](https://leetcode.com/problems/gas-station/) (greedy) | A 2025 final-interview report. |
| [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) | Same report |
| [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | A Power Programmer report. |
| [LIS](https://leetcode.com/problems/longest-increasing-subsequence/) | The 2026 offline report above |
| Largest square path in an R/L/U/D direction matrix | Same report (no known link) |
| [Number of Provinces](https://leetcode.com/problems/number-of-provinces/) / [Course Schedule](https://leetcode.com/problems/course-schedule/) | My mapping for "cycles + connected components" |
| [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | My mapping for "sorting + binary search" |
| [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) | My mapping for a "medium BST" asked live |
| [Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/) | A HackWithInfy interview report. |

## C. Repeating patterns

One prep video groups the repeats into 6 patterns: grid BFS/DFS, DP, arrays and intervals, strings, design, and quick basics.

- **Grid/graph:** [Number of Islands](https://leetcode.com/problems/number-of-islands/), [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/), [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) (premium), [Network Delay Time](https://leetcode.com/problems/network-delay-time/) (Dijkstra)
- **DP:** [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/), [Coin Change](https://leetcode.com/problems/coin-change/), [LCS](https://leetcode.com/problems/longest-common-subsequence/), [Edit Distance](https://leetcode.com/problems/edit-distance/), [Word Break](https://leetcode.com/problems/word-break/), [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/), [Target Sum](https://leetcode.com/problems/target-sum/)
- **Arrays/intervals:** [Insert Interval](https://leetcode.com/problems/insert-interval/), [Merge Intervals](https://leetcode.com/problems/merge-intervals/), [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/), [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/), [Next Greater Element](https://leetcode.com/problems/next-greater-element-i/), [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) (the Minimum Platforms idea)
- **Strings:** [Remove Adjacent Duplicates](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/), [Find the Closest Palindrome](https://leetcode.com/problems/find-the-closest-palindrome/) (a stand-in for "next palindrome"), [Largest Number](https://leetcode.com/problems/largest-number/), [Remove Digit to Maximize Result](https://leetcode.com/problems/remove-digit-from-number-to-maximize-result/)
- **Design:** [LRU Cache](https://leetcode.com/problems/lru-cache/), [Design HashMap](https://leetcode.com/problems/design-hashmap/)
- **Backtracking:** [N-Queens](https://leetcode.com/problems/n-queens/), [Combination Sum](https://leetcode.com/problems/combination-sum/)
- **Bits/basics:** [Single Number](https://leetcode.com/problems/single-number/), [Bulb Switcher](https://leetcode.com/problems/bulb-switcher/)

## D. HackWithInfy (same difficulty tier)

Naukri Code360 lists Beautiful Function, Paper Sheets, Special Prime and The Dishes Problem as past HackWithInfy problems. Link: [Round 2 most asked](https://www.naukri.com/code360/library/round-2-most-asked-questions-hwi)

## Sources to mine

- [PrepInsta main](https://prepinsta.com/infosys-sp-and-dse/coding-questions/), [SP](https://prepinsta.com/infosys-sp-and-dse/specialist-programmer/coding-questions/), [DSE](https://prepinsta.com/infosys-sp-and-dse/digital-specialist-engineer/coding-questions/)
- [GitHub prep sheet](https://github.com/bhasidhshaik/Infosys-SP-DSE-resources). It links a [Reddit thread](https://www.reddit.com/r/developersIndia/comments/1pmi730/infosys_specialist_programmer_coding_round_for/) and a [second repo](https://github.com/karthikreddy-7/Infosys-SP-Coding-Questions) that I couldn't open, so check them yourself.
- [LeetCode onsite report](https://leetcode.com/discuss/post/7233197/)
- [Video: the 6 patterns](https://www.youtube.com/watch?v=Ks5szuhId3c)

## Think, Plan, Execute

**Think:** Your target of 3–4 out of 4 is ambitious. The sources also say the online-test marks are weighted 20/30/50. The hard DP question is worth half, and it's where most people die.

**Plan:**
1. Lock the easy/medium tier first (arrays, hashing, sliding window, greedy, binary search). These are the questions you can't afford to miss.
2. Then the DP ladder in the order of section C.
3. Then table A rows 1–13 as timed practice.
4. Save rows 14–17 (the Codeforces-grade ones) for last. Don't burn your week on them.

**Execute:** Do every problem on paper or in a plain editor, with a timer, and say the time complexity out loud. If you can't re-write a solution from scratch the next day, you haven't learned it, you've just read it.

Want this as a tick-off checklist doc, ordered by priority?
