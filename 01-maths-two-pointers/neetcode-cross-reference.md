# NeetCode 150 Cross-Reference — Section 01 (Maths and Two Pointers)

Tracks which lectures in this section map to NeetCode 150 problems, and flags NeetCode problems in the same pattern space that the course doesn't cover directly — solve these separately as spot-checks.

## Direct Matches (course lecture == NeetCode problem)

| Course Lecture | LeetCode # | Title | Notes File |
|---|---|---|---|
| Two sum | 167 | Two Sum II - Input Array Is Sorted | `11-two-sum.md` |
| Container with most water | 11 | Container With Most Water | `12-container-with-most-water.md` |
| sort 0,1 and 2 | 75 | Sort Colors | `13-sort-0-1-2-dutch-national-flag.md` |

## Related NeetCode 150 Problems NOT in This Course Section — Solve These Too

| LeetCode # | Title | Why It's Relevant Here | Priority |
|---|---|---|---|
| 1 | Two Sum (unsorted, hashmap) | Same name as 167 but different technique — the #1 trap, see `11-two-sum.md` §6 | High — must be able to distinguish both |
| 15 | 3Sum | Direct extension of the sorted two-pointer pattern | High — asked constantly |
| 18 | 4Sum | Further extension, two nested fixed loops + two-pointer inner | Medium |
| 42 | Trapping Rain Water | Related to Container With Most Water but needs prefix/suffix max arrays, not just two pointers — will revisit when Arrays section covers it | Medium (flag for later, don't solve yet — Arrays section builds the prerequisite) |
| 125 | Valid Palindrome | Basic converging two-pointer, good warm-up if 167/11 felt shaky | Low — only if fundamentals need reinforcing |
| 11 → follow-up | — | Try re-deriving the "move shorter pointer" proof without looking at notes | — |

## Coverage Gaps to Be Aware Of
- **GCD/Sieve/Co-prime have almost no NeetCode 150 presence.** This is expected — NeetCode 150 is curated around common interview patterns, and pure number-theory problems are rarer in FAANG rounds than in this course (which leans more competitive-programming-flavored for the maths section). Treat GCD/Sieve as **utility tools** you might need *inside* a NeetCode-style problem, not standalone interview questions to drill repeatedly.
- If a mock interview or NeetCode session surfaces a GCD/factorization sub-problem unexpectedly, that's exactly when these notes pay off — the course covered the tool, NeetCode will occasionally test whether you can apply it.

## How to Use This File
When you finish a NeetCode 150 problem that overlaps with this section, come back and check it off / add a one-line note here (variation encountered, any gotcha) rather than creating a whole new file — this file is the index, the course-lecture files are the deep dives.

## Progress Checklist
- [ ] LeetCode 1 — Two Sum
- [ ] LeetCode 167 — Two Sum II
- [ ] LeetCode 11 — Container With Most Water
- [ ] LeetCode 75 — Sort Colors
- [ ] LeetCode 15 — 3Sum
- [ ] LeetCode 18 — 4Sum
- [ ] LeetCode 125 — Valid Palindrome (optional, only if needed)
