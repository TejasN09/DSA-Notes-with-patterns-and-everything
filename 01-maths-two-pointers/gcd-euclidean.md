# GCD — Euclidean Algorithm

**Course Section:** 02 — Maths and Two Pointers
**Lecture(s):** Intro to GCD, Find GCD of two numbers, GCD of entire array, Check if subsequence exists with GCD == 1, Delete to maximize, Number of subarrays with GCD == k, X of a kind in a deck
**Status:** ✅ Understood conceptually — pending: self-implement all variants cold, no reference

---

## 1. Priority
Medium-low as a standalone interview ask, **high as a hidden prerequisite**. Rarely asked as "write GCD" directly in FAANG rounds — but shows up buried inside harder problems (fraction simplification, cycle detection, coprime counting, partitioning). Not having this instant costs you interview minutes you need elsewhere.

## 2. Goal
Write GCD from memory in under 60 seconds. Instantly recognize when a problem is secretly a GCD problem (keywords below). Know the prefix/suffix GCD trick cold — it generalizes beyond GCD.

## 3. When to Spot It (signal phrases)
- "greatest common divisor" / "largest number that evenly divides"
- "simplify a fraction" / "reduce to smallest ratio"
- "common factor across an array"
- "co-prime" / "relatively prime"
- "cycle length" (in some graph/rotation problems)
- "partition into equal groups" (deck of cards style)

## 4. Core Idea
`gcd(a, b) = gcd(b, a % b)`, until `b == 0`, then answer is `a`.

**Why it works:** if `d` divides both `a` and `b`, then `d` also divides `a - b`, and therefore `a % b` (repeated subtraction). So common divisors of `(a, b)` == common divisors of `(b, a % b)`. The problem shrinks without losing information. Since `a % b < b` always, shrinkage is fast — worst case is consecutive Fibonacci numbers, giving O(log(min(a,b))).

## 5. Template (memorize exactly)

**Recursive:**
```java
static int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

**Iterative (safer — no stack overflow on large inputs):**
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

**Key identity to remember:** `gcd(x, 0) = x` — avoids special-casing edges in prefix/suffix problems.

## 6. Worked Example
`gcd(48, 18)`
- `gcd(48, 18)` → `48 % 18 = 12` → `gcd(18, 12)`
- `gcd(18, 12)` → `18 % 12 = 6` → `gcd(12, 6)`
- `gcd(12, 6)` → `12 % 6 = 0` → `gcd(6, 0)` → return **6**

## 7. Problem Variations

### 7.1 GCD of entire array
```java
int result = arr[0];
for (int i = 1; i < arr.length; i++) {
    result = gcd(result, arr[i]);
    if (result == 1) break; // GCD can't go below 1 — early exit
}
```

### 7.2 Does a subsequence exist with GCD == 1?
**Insight:** GCD of a set is monotonically non-increasing as you add elements. So GCD of the *entire array* is the minimum achievable by any subset. → Just check `gcd(all elements) == 1`.

### 7.3 Delete one element to maximize array GCD
Naive: O(n²) — remove each element, recompute.
**Optimal — prefix/suffix GCD arrays (O(n)):**
```java
int[] prefix = new int[n], suffix = new int[n];
prefix[0] = arr[0];
for (int i = 1; i < n; i++) prefix[i] = gcd(prefix[i-1], arr[i]);
suffix[n-1] = arr[n-1];
for (int i = n-2; i >= 0; i--) suffix[i] = gcd(suffix[i+1], arr[i]);

int best = 0;
for (int i = 0; i < n; i++) {
    int left = (i == 0) ? 0 : prefix[i-1];
    int right = (i == n-1) ? 0 : suffix[i+1];
    best = Math.max(best, gcd(left, right)); // gcd(x,0)=x handles edges
}
```
⭐ **This prefix/suffix trick is reusable far beyond GCD** — same shape solves "max product after removing one element," "max sum after removing one element," etc. Worth treating as its own standalone pattern, not a GCD footnote.

### 7.4 Count subarrays with GCD == k
- Brute O(n²): for each start index, extend right, maintain running GCD (update incrementally, don't recompute from scratch), count matches.
- **Reduction trick:** divide every element by `k` first (skip elements not divisible by `k` — they break the subarray). Now count subarrays with GCD exactly `1` in each valid segment — reduces back to pattern 7.2. Recognizing this reduction is the real interview skill.

### 7.5 X of a Kind in a Deck of Cards
Count frequency of each card value → take GCD of all frequencies → answer is `gcd >= 2`.
**Meta-skill:** recognizing when the real ask ("can I partition into equal groups") is secretly "GCD of a *derived* array (frequencies), not the raw input."

## 8. Gotchas / Edge Cases
- `gcd(x, 0) = x`, not undefined — use this, don't special-case it.
- GCD is monotonic non-increasing as a set grows — this underlies half the variant problems above.
- Overflow: if inputs can be large, watch `int` vs `long` in the modulo op.
- Recursive version can stack-overflow on pathological/very large inputs — prefer iterative in production interview code unless recursion depth is clearly bounded.

## 9. Complexity
- Time: O(log(min(a, b))) per pair.
- Space: O(1) iterative, O(log(min(a,b))) recursive (call stack).
- Array-wide GCD: O(n log(max element)).

## 10. Related Patterns
- Sieve of Eratosthenes (factorization) — see `sieve-of-eratosthenes.md`
- Co-prime check — just `gcd(a,b) == 1`, no separate algorithm needed.
- Prefix/Suffix aggregation trick — generalizes to sum, product, max, min, not just GCD.

## 11. Practice / Interview Questions
1. Count pairs `(i, j)` in an array where `gcd(arr[i], arr[j]) == 1`. (Brute O(n²) is often the *expected* answer — don't over-engineer unless n is large.)
2. Delete at most one element to maximize GCD of the rest. (→ 7.3, prefix/suffix GCD)
3. Given frequency constraints, can you split a deck into groups of equal size with matching values? (→ 7.5)

## 12. My Notes / Confusion Points
*(fill in as you go — anything that tripped you up during the lecture or while practicing)*

## 13. Links
- Course: Udemy — Advanced DSA for Interview-level Problem Solving [Java], Section 2
