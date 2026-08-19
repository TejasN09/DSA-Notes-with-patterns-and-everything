# Delete One Element to Maximize Array GCD

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Delete to maximize
**NeetCode 150:** Not present directly, but the **prefix/suffix aggregation trick** used here is the same one behind NeetCode's "Product of Array Except Self" (Arrays section, see `07-product-of-array-except-itself.md` once we get there) — recognize this as one reusable technique wearing different costumes.
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an array of `n` positive integers, you may delete **exactly one** element. Maximize the GCD of the remaining `n-1` elements.

## 2. Approach

**Brute force:** for each index `i`, remove it, recompute GCD of the rest from scratch. O(n) removals × O(n log(max)) GCD computation = **O(n² log(max))**. Too slow for large `n`.

**Optimal — Prefix/Suffix GCD arrays, O(n):**
Precompute:
- `prefix[i]` = GCD of `arr[0..i]`
- `suffix[i]` = GCD of `arr[i..n-1]`

Then for each index `i` (the element we're considering removing), the GCD of everything *except* index `i` is:
`gcd(prefix[i-1], suffix[i+1])`

Take the max over all `i`. Using the identity `gcd(x, 0) = x` lets you handle the boundary cases (`i == 0` or `i == n-1`) without special-casing — just treat the missing side as `0`.

## 3. Code
```java
static int deleteToMaximize(int[] arr) {
    int n = arr.length;
    int[] prefix = new int[n];
    int[] suffix = new int[n];

    prefix[0] = arr[0];
    for (int i = 1; i < n; i++) prefix[i] = gcd(prefix[i - 1], arr[i]);

    suffix[n - 1] = arr[n - 1];
    for (int i = n - 2; i >= 0; i--) suffix[i] = gcd(suffix[i + 1], arr[i]);

    int best = 0;
    for (int i = 0; i < n; i++) {
        int left = (i == 0) ? 0 : prefix[i - 1];
        int right = (i == n - 1) ? 0 : suffix[i + 1];
        best = Math.max(best, gcd(left, right)); // gcd(x,0)=x handles edges
    }
    return best;
}
```

## 4. Dry Run
`arr = [4, 8, 12, 6]`

Prefix: `[4, gcd(4,8)=4, gcd(4,12)=4, gcd(4,6)=2]` → `[4, 4, 4, 2]`
Suffix: `[gcd(4,gcd(8,gcd(12,6))), gcd(8,gcd(12,6)), gcd(12,6), 6]`
  - `suffix[3] = 6`
  - `suffix[2] = gcd(12, 6) = 6`
  - `suffix[1] = gcd(8, 6) = 2`
  - `suffix[0] = gcd(4, 2) = 2`
  → `[2, 2, 6, 6]`

Now try removing each index:
- Remove idx 0: `gcd(0, suffix[1]=2) = 2`
- Remove idx 1: `gcd(prefix[0]=4, suffix[2]=6) = 2`
- Remove idx 2: `gcd(prefix[1]=4, suffix[3]=6) = 2`
- Remove idx 3: `gcd(prefix[2]=4, 0) = 4`

Best = **4** (achieved by removing index 3, the `6`) — remaining `[4,8,12]` has GCD 4. ✓ Correct — removing the element that's "dragging down" the overall GCD gives the best result, and the prefix/suffix scan finds it in one O(n) pass.

## 5. Complexity
- Time: O(n log(max element)) — one pass to build prefix, one for suffix, one to combine.
- Space: O(n) for the two auxiliary arrays.

## 6. Edge Cases
- `n == 2`: removing either element just leaves the other single element as the "GCD" (a single number's GCD with nothing is itself) — the `gcd(x, 0) = x` identity handles this cleanly.
- All elements identical: answer is that value regardless of which one you remove.
- Removing the "wrong" index never helps — the algorithm naturally finds the best regardless, no need to reason about which element to target manually.

## 7. ⭐ Why This Trick Deserves Its Own Mental Category
The prefix/suffix aggregation pattern — precompute a left-to-right fold and a right-to-left fold, then combine at each index to answer "what if I removed/excluded position `i`?" — is **not GCD-specific**. The exact same shape solves:
- Product of array except self (multiply instead of gcd)
- Max sum after removing one element (sum instead of gcd)
- Max/min after excluding one element in general

**Recognize the shape:** "excluding one position, aggregate the rest, optimize" → prefix/suffix arrays, O(n), every time.

## 8. Related Patterns
- Builds on `01-gcd-fundamentals.md`, `02-gcd-of-array.md`
- Same technique reused later in Arrays section (Product of Array Except Itself)

## 9. My Notes / Confusion Points

