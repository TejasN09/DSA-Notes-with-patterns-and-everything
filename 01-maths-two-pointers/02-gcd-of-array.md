# GCD of Entire Array

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** GCD of entire array
**NeetCode 150:** Not directly present — utility building block for other problems.
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an array of `n` positive integers, find the GCD of all elements.

## 2. Approach
Fold `gcd()` across the array left to right — GCD is associative, so `gcd(a,b,c) = gcd(gcd(a,b), c)`, order doesn't matter.

**Optimization:** GCD is monotonically non-increasing as you fold in more elements (adding a number can only keep the GCD the same or shrink it, never grow it). The moment running GCD hits `1`, it can never go lower (1 is the smallest possible positive GCD) — break immediately.

## 3. Code
```java
static int gcdOfArray(int[] arr) {
    int result = arr[0];
    for (int i = 1; i < arr.length; i++) {
        result = gcd(result, arr[i]);
        if (result == 1) break; // can't shrink below 1, early exit
    }
    return result;
}
```

## 4. Dry Run
`arr = [12, 8, 32, 4]`
- `result = 12`
- `gcd(12, 8) = 4` → result = 4
- `gcd(4, 32) = 4` → result = 4
- `gcd(4, 4) = 4` → result = 4
- Answer: **4**

`arr = [7, 5, 3]` (will hit early exit)
- `result = 7`
- `gcd(7, 5) = 1` → result = 1 → **break immediately**, skip remaining elements
- Answer: **1**

## 5. Complexity
- Without early exit: O(n log(max element))
- With early exit: same worst case, but much faster average case on arrays that quickly reach GCD 1 (common in random data)
- Space: O(1)

## 6. Edge Cases
- Single-element array → GCD is that element itself.
- All elements identical → GCD is that value.
- Array contains a `1` → GCD is immediately 1 (fold from there triggers early exit right away).

## 7. Related Patterns
- Builds directly on `01-gcd-fundamentals.md`
- Prerequisite for `03-subsequence-with-gcd-equal-1.md` and `04-delete-to-maximize.md`

## 8. My Notes / Confusion Points