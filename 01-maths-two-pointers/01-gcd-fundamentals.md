# GCD Fundamentals — Intro + Euclidean Algorithm

**Course Section:** 02 — Maths and Two Pointers
**Lecture(s):** Intro to GCD, Find GCD of two numbers
**NeetCode 150:** Not directly covered (NeetCode doesn't have a standalone GCD lecture) — but GCD is a prerequisite utility used inside NeetCode-style problems (fraction reduction, cycle detection). Treat this file as a utility reference, not a standalone interview question.
**Status:** ✅ Understood — pending self-implementation cold, no reference

---

## 1. Problem Statement
Given two positive integers `a` and `b`, find the **G**reatest **C**ommon **D**ivisor — the largest integer that divides both `a` and `b` without remainder.

## 2. Priority & Why It Matters
Medium-low as a standalone ask, **high as a hidden prerequisite**. Rarely asked directly as "write GCD" in FAANG rounds, but shows up buried inside: fraction simplification, cycle-length problems, coprime-pair counting, array partitioning (see later files in this section). If this isn't automatic, you burn interview time deriving it instead of solving the actual problem.

## 3. When to Spot It (signal phrases)
- "greatest common divisor" / "largest number that evenly divides"
- "simplify a fraction" / "reduce to lowest terms"
- "common factor"
- Anywhere the word "coprime" or "relatively prime" appears

## 4. Approach / Core Idea
**Brute force (don't use this, but know why it's wrong):** check every number from `min(a,b)` down to 1, return first that divides both. O(min(a,b)) — too slow for large inputs.

**Optimal — Euclidean Algorithm:** `gcd(a, b) = gcd(b, a % b)`, repeat until `b == 0`, then answer is `a`.

**Why it works:** if `d` divides both `a` and `b`, then `d` also divides `a - b`, and by extension `a % b` (since `%` is just repeated subtraction of `b` from `a`). So the *set* of common divisors of `(a, b)` is identical to the set of common divisors of `(b, a % b)` — you shrink the problem without losing any information. Since `a % b < b` strictly, the numbers shrink fast. Worst case (slowest shrinkage) happens on consecutive Fibonacci numbers, which bounds the algorithm at O(log(min(a,b))).

## 5. Code

**Recursive (clean, but uses call stack):**
```java
static int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

**Iterative (preferred in interviews — no stack overflow risk):**
```java
static int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

**Key identity — memorize this, it removes edge-case handling everywhere downstream:**
`gcd(x, 0) = x`

## 6. Dry Run
`gcd(48, 18)`
| Step | a | b | a % b |
|---|---|---|---|
| 1 | 48 | 18 | 12 |
| 2 | 18 | 12 | 6 |
| 3 | 12 | 6 | 0 |
| return | 6 | — | — |

`gcd(48, 18) = 6`. Check: 48 = 6×8, 18 = 6×3. ✓

## 7. Complexity
- Time: O(log(min(a, b)))
- Space: O(1) iterative, O(log(min(a,b))) recursive (call stack depth)

## 8. Edge Cases
- `gcd(a, 0) = a` — don't special case, the algorithm handles it naturally if you start the loop correctly.
- `gcd(0, 0)` is mathematically undefined — most implementations return 0; clarify with interviewer if this input is possible.
- Negative numbers: GCD is conventionally defined for non-negative integers — if inputs can be negative, take `Math.abs()` first.
- Overflow: `a % b` on `int` is safe as long as inputs fit in `int`; if inputs approach `Integer.MAX_VALUE` and you're doing repeated multiplication elsewhere (e.g. LCM = a*b/gcd(a,b)), watch for overflow — use `long`.

## 9. LCM (related, often asked together)
`lcm(a, b) = (a * b) / gcd(a, b)` — divide first or use `long` to avoid overflow:
```java
static long lcm(int a, int b) {
    return (long) a / gcd(a, b) * b;
}
```

## 10. Related Patterns / Next Files
- `02-gcd-of-array.md` — folding GCD across an array
- `03-subsequence-with-gcd-equal-1.md`
- `04-delete-to-maximize.md`
- `05-count-subarray-gcd-k.md`
- `06-x-of-a-kind-in-deck-of-cards.md`
- `07-sieve-of-eratosthenes.md` (factorization — different tool, same "maths" family)
- `10-co-prime.md` — direct application of this file

## 11. My Notes / Confusion Points