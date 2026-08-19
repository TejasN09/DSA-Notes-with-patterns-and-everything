# Sieve of Eratosthenes (+ Smallest Prime Factor variant)

**Course Section:** 02 — Maths and Two Pointers
**Lecture(s):** Sieve of Eratosthenes, Smallest prime factor of all numbers, Factors for multiple queries, Co-prime
**Status:** ✅ Understood conceptually — pending: self-implement SPF sieve cold, no reference

---

## 1. Priority
**Higher than plain GCD.** Sieve — especially the Smallest Prime Factor (SPF) variant — is a genuine building block in medium/hard problems requiring fast repeated factorization. "Multiple queries" is the strongest signal this pattern is expected.

## 2. Goal
Write standard sieve and SPF sieve from memory. Recognize "many queries about primes/factors up to N" as an instant precompute-once signal.

## 3. When to Spot It (signal phrases)
- "prime numbers up to N"
- "multiple queries about primality / factors" ← strongest signal for SPF specifically
- "count primes"
- "smallest prime factor"
- "prime factorization — efficiently, many times"

## 4. Core Idea
Instead of checking each number for primality individually (O(√n) each → O(n√n) total), mark multiples of every prime starting from 2. Each composite gets crossed off by its smallest prime factor, so total work across all primes sums to O(n log log n) — near-linear.

## 5. Template

**Standard sieve (primality only):**
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

**Two things that separate "knows sieve" from "actually understands it":**
1. Inner loop starts at `i * i`, not `2 * i` — anything below `i²` with factor `i` was already marked by a smaller prime.
2. Use `boolean[]` not `int[]` — 4x less memory, matters when n is 10^7+.

**Smallest Prime Factor (SPF) sieve — the upgrade for fast repeated factorization:**
```java
static int[] smallestPrimeFactor(int n) {
    int[] spf = new int[n + 1];
    for (int i = 2; i <= n; i++) {
        if (spf[i] == 0) { // i is prime
            for (int j = i; j <= n; j += i) {
                if (spf[j] == 0) spf[j] = i;
            }
        }
    }
    return spf;
}

// Factorize any number <= n in O(log n) after precompute:
static List<Integer> factorize(int x, int[] spf) {
    List<Integer> factors = new ArrayList<>();
    while (x > 1) {
        factors.add(spf[x]);
        x /= spf[x];
    }
    return factors;
}
```
Precompute once: O(n log log n). Every factorization query after that: O(log n) instead of O(√n). Classic time-space tradeoff — **this is the answer whenever a problem says "you'll be asked to factorize many numbers up to N."**

## 6. Worked Example
`sieve(30)` marks composites: 4,6,8,9,10,12,14,15,16,18,20,21,22,24,25,26,27,28,30.
Primes remaining: 2,3,5,7,11,13,17,19,23,29.

`spf` for 60: `spf[60] = 2` → `60/2=30` → `spf[30]=2` → `30/2=15` → `spf[15]=3` → `15/3=5` → `spf[5]=5` → `5/5=1`, stop.
Factorization of 60: `[2, 2, 3, 5]` → matches `60 = 2² × 3 × 5`. ✓

## 7. Co-prime (not a separate algorithm)
Two numbers are co-prime iff `gcd(a, b) == 1` — just apply the GCD pattern (see `gcd-euclidean.md`). "Co-prime pairs" problems are asking you to combine GCD checks with a counting/pairing structure (often hashmap of value → frequency, then GCD-check across distinct values).
**Trap:** don't reach for sieve/factorization here unless the problem specifically needs prime factors — co-prime is a property check, not a factorization problem.

## 8. Gotchas / Edge Cases
- `(long) i * i` in the loop condition — avoids `int` overflow when `i` is large and `n` is near `Integer.MAX_VALUE`.
- `spf[i] == 0` check when marking — don't overwrite an already-set SPF; you want the *smallest* prime factor, and primes are processed in increasing order, so first write is always correct — but the guard still matters for correctness/clarity.
- Sieve gives primality up to n; SPF sieve gives you factorization up to n — pick based on what the problem actually needs, don't default to the heavier one.

## 9. Complexity
- Standard sieve: O(n log log n) time, O(n) space.
- SPF sieve: O(n log log n) precompute, O(log n) per factorization query after that, O(n) space.

## 10. Related Patterns
- GCD / Euclidean algorithm — see `gcd-euclidean.md`
- Precompute-once-query-many is a general interview signal, not sieve-specific — same instinct applies to prefix sums, DP memoization tables, etc.

## 11. Practice / Interview Questions
1. `n` up to 10^6, `q` up to 10^5 queries each asking "how many prime factors does x have (with multiplicity)?" → SPF sieve, then O(log x) per query.
2. Count all primes ≤ N. → standard sieve, count `false` entries.
3. Given a number, return its full prime factorization efficiently across many calls. → SPF sieve + factorize().

## 12. My Notes / Confusion Points
*(fill in as you go)*

## 13. Links
- Course: Udemy — Advanced DSA for Interview-level Problem Solving [Java], Section 2
