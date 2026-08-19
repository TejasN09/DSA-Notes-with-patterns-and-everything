# Two Sum (Two-Pointer / Sorted Array Variant)

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Two sum
**NeetCode 150:** LeetCode 1 "Two Sum" (hashmap variant — unsorted, return indices) AND LeetCode 167 "Two Sum II - Input Array Is Sorted" (this exact two-pointer variant). **These are two different problems that look identical — see Section 6 below, this is the #1 trap in this topic.**
**Status:** ✅ Understood

---

## 1. Problem Statement (sorted variant — what this lecture covers)
Given a **sorted** array of integers and a target value, find two numbers that add up to the target. Return their values (or 1-indexed positions, per LeetCode 167 convention).

## 2. When to Spot It (signal phrases)
- "sorted array" + "find a pair that sums to X"
- If array isn't sorted but you're free to sort it (and original indices don't matter) — sort first, then apply this pattern.

## 3. Approach
Two pointers starting at opposite ends: `left = 0`, `right = n-1`.
- If `arr[left] + arr[right] == target` → found it.
- If sum is **too small** → need a bigger sum → move `left++` (moving right would only decrease or keep sum the same, since array is sorted ascending).
- If sum is **too large** → move `right--`.

**Why this is correct (the proof interviewers want to hear):** at each step, if `arr[left] + arr[right] < target`, then `arr[left]` paired with *any* element ≤ `arr[right]` is also too small (array is sorted, so all those pairs are ≤ current sum). That means `arr[left]` can never be part of a valid pair with anything at or before `right`'s current position — safe to permanently discard it and move `left++`. Symmetric argument for the other direction. Every step eliminates at least one candidate pair from consideration, and no valid pair is ever skipped.

## 4. Code
```java
static int[] twoSumSorted(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return new int[]{left, right};
        else if (sum < target) left++;
        else right--;
    }
    return new int[]{-1, -1}; // no pair found
}
```

## 5. Dry Run
`arr = [2, 7, 11, 15]`, `target = 9`
- `left=0, right=3`: `2+15=17 > 9` → `right--`
- `left=0, right=2`: `2+11=13 > 9` → `right--`
- `left=0, right=1`: `2+7=9 == 9` → **found**, return `[0, 1]`

## 6. ⚠️ THE TRAP — Two Sum I vs Two Sum II
This is the single most important thing to internalize from this lecture:

| | LeetCode 1 (Two Sum) | LeetCode 167 (Two Sum II) |
|---|---|---|
| Input | **Unsorted** array | **Sorted** array |
| Return | Original **indices** | Values (or 1-indexed positions in the sorted array) |
| Technique | **Hashmap** (value → index), O(n) time, O(n) space | **Two pointers**, O(n) time, O(1) space |
| Why not two-pointer on #1 | Sorting would destroy original index information | N/A — sorted order is directly usable |
| Why not hashmap on #167 | Works too, but two-pointer is the "intended" O(1)-space answer | N/A |

**Interview move:** the moment you see "Two Sum," clarify: *is the array sorted, and do I need to return original indices?* That single question determines whether you reach for a hashmap or two pointers — asking it upfront signals real understanding, not just pattern memorization.

## 7. Complexity
- Two-pointer version: O(n) time (already sorted), O(1) space.
- If you need to sort first: O(n log n) time.

## 8. Edge Cases
- No valid pair exists → return sentinel / empty (clarify expected behavior with interviewer).
- Duplicate values → still works correctly, pointers just point at equal or different indices holding the same value.
- Target achieved at `left == right` isn't valid (same element used twice) — the `while (left < right)` condition already prevents this.

## 9. Related Patterns
- Extends directly to **3Sum**: fix one element with an outer loop, two-pointer the rest of the sorted array for the remaining two. (LeetCode 15, NeetCode 150)
- Extends to **4Sum** similarly (two nested fixed loops + two-pointer inner).
- Same converging-pointer mechanism as `12-container-with-most-water.md`.

## 10. NeetCode 150 Cross-Reference — Do These Too
- LeetCode 1 — Two Sum (hashmap variant, make sure you can do **both** versions)
- LeetCode 167 — Two Sum II (this lecture, exactly)
- LeetCode 15 — 3Sum (direct extension, high priority — asked constantly)
- LeetCode 18 — 4Sum (further extension, lower priority but good practice)

## 11. My Notes / Confusion Points

