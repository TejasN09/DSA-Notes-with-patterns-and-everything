# Sieve of Eratosthenes

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Sieve of Eratosthenes
**NeetCode 150 / LeetCode:** LeetCode 204 — "Count Primes" is the canonical sieve problem; not in NeetCode 150's core list but extremely common as a warm-up/utility question and a building block for other problems.
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an integer `n`, find all prime numbers ≤ `n` efficiently (equivalently: count them, as in LeetCode 204).

## 2. Approach

**Brute force:** for each number `x` from 2 to `n`, check primality by trial division up to `√x`. O(n√n) total — too slow for large `n` (e.g. n = 10^7).

**Optimal — Sieve of Eratosthenes:** instead of checking each number individually, mark off multiples of every prime as composite, starting from 2. Every composite number gets marked by its smallest prime factor, so the total work across all primes sums to O(n log log n) — very close to linear.

## 3. Code
```java
static boolean[] sieve(int n) {
    boolean[] isComposite = new boolean[n + 1];
    for (int i = 2; (long) i * i <= n; i++) {
        if (!isComposite[i]) {
            for (int j = i * i; j <= n; j += i) {
                isComposite[j] = true;
            }
        }
    }
    return isComposite; // isComposite[x] == false means x is prime (for x >= 2)
}
```

**Two details that separate "knows sieve" from "actually understands it" — say these out loud in an interview:**
1. **Inner loop starts at `i * i`, not `2 * i`.** Any composite smaller than `i²` with factor `i` was already marked by a *smaller* prime factor earlier in the outer loop. Starting at `i*i` avoids redundant work.
2. **Use `boolean[]`, not `int[]`.** 4x less memory — matters a lot when `n` is 10^7+ and memory limits are tight.
3. **`(long) i * i` in the loop condition** — prevents `int` overflow when `i` is large and close to `√Integer.MAX_VALUE`.

## 4. Dry Run
`sieve(30)`:
- Start: all `false` (assume prime)
- `i=2`: mark 4,6,8,10,12,14,16,18,20,22,24,26,28,30 as composite
- `i=3`: mark 9,12,15,18,21,24,27,30 (some already marked, harmless)
- `i=4`: already composite, skip
- `i=5`: `5*5=25 <= 30`, mark 25, 30
- `i=6`: `6*6=36 > 30`, loop ends

Primes remaining (not marked composite): **2, 3, 5, 7, 11, 13, 17, 19, 23, 29**

## 5. Complexity
- Time: O(n log log n) — very close to linear in practice
- Space: O(n)

## 6. Edge Cases
- `n < 2` → no primes exist, return appropriately (empty result or all-composite array).
- `n = 2` → smallest edge case with one prime.
- Watch off-by-one on array size — need indices `0..n` inclusive, so array size is `n+1`.

## 7. Related Patterns
- Foundation for `08-smallest-prime-factor.md` (the upgraded version for repeated factorization)
- Distinct tool from GCD — sieve is about *finding/marking primes*, GCD is about *common divisors*. Don't conflate; they solve different sub-problems even though both fall under "maths" in this course.

## 8. My Notes / Confusion Points

