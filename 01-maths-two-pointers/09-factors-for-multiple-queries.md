# Factors for Multiple Queries

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Factors for multiple queries
**NeetCode 150:** Not present — competitive-programming-style optimization problem, but the "precompute once, answer many queries fast" instinct is broadly interview-relevant.
**Status:** ✅ Understood

---

## 1. Problem Statement
Given `n` (upper bound on values) and `q` queries, each asking for something about a number's factors (e.g. "how many divisors does x have", "list all prime factors of x", "sum of prime factors with multiplicity") — answer all `q` queries efficiently, where naive per-query computation would be too slow.

## 2. Why Naive Fails
Per-query trial division factorization is O(√x). With `q` up to 10^5 and `x` up to 10^6, that's up to 10^5 × 10^3 = 10^8 operations — borderline, and gets worse if `x` is larger or `q` is bigger. The moment you see **"multiple queries"** + **"factorization"** together, that's the signal to precompute.

## 3. Approach
1. Precompute the **Smallest Prime Factor (SPF)** array for all values up to `n` once — O(n log log n). See `08-smallest-prime-factor.md`.
2. For each query, factorize using SPF in O(log x) by repeatedly dividing by `spf[x]`.
3. From the prime factorization, derive whatever the query actually asks:
   - **Number of divisors:** if `x = p1^e1 * p2^e2 * ... * pk^ek`, divisor count = `(e1+1)(e2+1)...(ek+1)`.
   - **Sum of divisors:** product of `(p_i^0 + p_i^1 + ... + p_i^ei)` for each prime.
   - **List of distinct prime factors:** just dedupe the factorize() output.
   - **Euler's totient, etc.:** derivable from the same prime factorization.

## 4. Code
```java
// Precompute once
int[] spf = smallestPrimeFactor(n); // from 08-smallest-prime-factor.md

// Answer each query in O(log x)
static int countDivisors(int x, int[] spf) {
    Map<Integer, Integer> primeExponents = new HashMap<>();
    while (x > 1) {
        int p = spf[x];
        primeExponents.merge(p, 1, Integer::sum);
        x /= p;
    }
    int divisorCount = 1;
    for (int exp : primeExponents.values()) {
        divisorCount *= (exp + 1);
    }
    return divisorCount;
}
```

## 5. Dry Run
`n = 100`, query: "how many divisors does 60 have?"
- Precompute SPF for 1..100 once.
- Factorize 60: `60 = 2² × 3¹ × 5¹` → exponents `{2:2, 3:1, 5:1}`
- Divisor count = `(2+1)(1+1)(1+1) = 3×2×2 = 12`
- Verify: divisors of 60 are 1,2,3,4,5,6,10,12,15,20,30,60 → 12 divisors ✓

## 6. Complexity
- Precompute: O(n log log n), done **once** regardless of `q`.
- Per query: O(log x) for factorization + O(log x) for aggregating exponents (small map) ≈ O(log x).
- Total for `q` queries: O(n log log n + q log n) — dramatically better than O(q √n) for large `q`.

## 7. Edge Cases
- Queries with `x` outside the precomputed range `[2, n]` → either reject or extend the sieve range; clarify constraints upfront with interviewer.
- `x = 1` → no prime factors, divisor count is 1 by convention — handle explicitly, the factorize loop naturally terminates immediately since `x > 1` is false from the start.
- Repeated queries for the same `x` → since SPF is precomputed and factorization is fast, no need for additional query-level memoization unless `q` is extremely large.

## 8. ⭐ The Actual Interview Skill Being Tested
This lecture isn't really about a new algorithm — it's about **recognizing the precompute-vs-per-query tradeoff.** The general pattern: "expensive operation, done many times, bounded input range" → **precompute once, amortize the cost.** Same instinct applies to prefix sums (Section 03), DP memoization tables, and hash-based lookups elsewhere in this course. Naming this tradeoff explicitly in an interview (rather than just coding SPF silently) signals you understand *why*, not just *how*.

## 9. Related Patterns
- Depends entirely on `08-smallest-prime-factor.md`
- General precompute-once instinct reused throughout the course

## 10. My Notes / Confusion Points

