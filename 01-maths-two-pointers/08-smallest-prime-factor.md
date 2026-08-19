# Smallest Prime Factor (SPF) Sieve

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Smallest prime factor of all numbers
**NeetCode 150:** Not present — this is a more advanced/competitive-programming-flavored technique, but shows up in LeetCode hard problems requiring repeated factorization (e.g. counting problems over divisor structures).
**Status:** ✅ Understood

---

## 1. Problem Statement
Given `n`, precompute the **smallest prime factor** of every integer from `2` to `n`, so that any individual number in that range can be factorized quickly afterward.

## 2. Approach
Same sieve mechanism as `07-sieve-of-eratosthenes.md`, but instead of a boolean "is composite" flag, store *which* prime first marked each number. Since the outer loop processes primes in increasing order, the **first** time a number gets marked, it's marked by its smallest prime factor — guaranteed by construction.

## 3. Code
```java
static int[] smallestPrimeFactor(int n) {
    int[] spf = new int[n + 1];
    for (int i = 2; i <= n; i++) {
        if (spf[i] == 0) { // i has not been marked yet → i is prime
            for (int j = i; j <= n; j += i) {
                if (spf[j] == 0) spf[j] = i; // only set if not already set
            }
        }
    }
    return spf;
}
```

**Why `if (spf[j] == 0)` matters:** without this guard, a later (larger) prime could overwrite an earlier, smaller, correct SPF value. Primes are processed in increasing order, so the first write for any `j` is always its true smallest prime factor — the guard preserves that.

## 4. Using SPF to factorize a number in O(log n)
```java
static List<Integer> factorize(int x, int[] spf) {
    List<Integer> factors = new ArrayList<>();
    while (x > 1) {
        factors.add(spf[x]);
        x /= spf[x];
    }
    return factors;
}
```
Each division at least halves `x` in the worst case (smallest prime factor is at least 2), so this terminates in O(log x) steps.

## 5. Dry Run
`smallestPrimeFactor(30)`:
- `i=2` (prime): mark spf[2,4,6,...,30] = 2 (only where unset)
- `i=3` (prime): mark spf[3,9,15,21,27] = 3 (6,12,18,24,30 already have spf=2, skipped)
- `i=4`: already set (spf[4]=2), skip
- `i=5` (prime): mark spf[5,25] = 5 (10,15,20,30 already set)
- ...continues

`factorize(60, spf)`:
- `spf[60] = 2` → add 2, `x = 30`
- `spf[30] = 2` → add 2, `x = 15`
- `spf[15] = 3` → add 3, `x = 5`
- `spf[5] = 5` → add 5, `x = 1`, stop

Result: `[2, 2, 3, 5]` → `60 = 2² × 3 × 5` ✓

## 6. Complexity
- Precompute: O(n log log n) time, O(n) space — same as standard sieve.
- Per-query factorization after precompute: **O(log n)**, vs. O(√n) per number without precompute.

## 7. When This Matters — "Factors for Multiple Queries" (next file)
The entire point of building SPF instead of a plain sieve is **amortizing cost across many queries.** If you're asked to factorize a number once, trial division (O(√x)) is simpler and fine. If you're told "you'll be asked to factorize many numbers, all ≤ n" — that's the signal to precompute SPF once (O(n log log n)) and answer each query in O(log n) after that. See `09-factors-for-multiple-queries.md`.

## 8. Edge Cases
- `spf[0]` and `spf[1]` are meaningless (undefined) — array indices 0 and 1 should never be queried; guard against it if the problem allows those inputs.
- Very large `n` → memory becomes the constraint (O(n) ints), not time.

## 9. Related Patterns
- Direct extension of `07-sieve-of-eratosthenes.md`
- Precompute-once-query-many is a general interview signal, reused elsewhere (prefix sums, DP memo tables) — not sieve-specific.

## 10. My Notes / Confusion Points

