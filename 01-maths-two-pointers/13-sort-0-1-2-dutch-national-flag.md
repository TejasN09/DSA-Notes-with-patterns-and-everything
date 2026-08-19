# Sort 0, 1, and 2 (Dutch National Flag Algorithm)

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** sort 0,1 and 2
**NeetCode 150:** LeetCode 75 — "Sort Colors" (exact match, core NeetCode 150 problem)
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an array containing only the values `0`, `1`, and `2`, sort it **in-place**, in **one pass**, using **O(1) extra space**. (Named "Sort Colors" on LeetCode — 0/1/2 represent red/white/blue.)

## 2. When to Spot It (signal phrases)
- "array with only [small fixed set of values]"
- "sort in-place, one pass, constant space"
- "three-way partition"
- More generally: any "partition array into 3 known regions" problem (not just 0/1/2 — e.g. negatives/zeros/positives)

## 3. Approach — Dutch National Flag (Dijkstra's algorithm)
Three pointers: `low`, `mid`, `high`.
- **Invariant maintained at every step:**
  - `arr[0 .. low-1]` = all 0s (finalized)
  - `arr[low .. mid-1]` = all 1s (finalized)
  - `arr[mid .. high]` = unexamined (still being processed)
  - `arr[high+1 .. n-1]` = all 2s (finalized)

```java
static void sortColors(int[] arr) {
    int low = 0, mid = 0, high = arr.length - 1;
    while (mid <= high) {
        if (arr[mid] == 0) {
            swap(arr, low, mid);
            low++; mid++;
        } else if (arr[mid] == 1) {
            mid++;
        } else { // arr[mid] == 2
            swap(arr, mid, high);
            high--;
            // do NOT increment mid — the swapped-in value from `high` is unexamined
        }
    }
}

static void swap(int[] arr, int i, int j) {
    int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
}
```

**Why the `2` case doesn't increment `mid`:** the value swapped in from `high` hasn't been checked yet — it could be a 0, 1, or another 2. If you incremented `mid` here too, you might skip over an unexamined element and break the invariant. In the `0` case, incrementing `mid` alongside `low` is safe because the value swapped in from `low` position was *already confirmed* to be part of the finalized-1s region or is the start of unexamined territory — actually the cleaner way to remember it: **the `0` swap brings a known-safe value (from the already-passed `low..mid-1` region, which was all 1s, or the very first unexamined element) into `mid`'s old spot, so advancing past it is fine. The `2` swap brings in a completely unknown value, so you must re-examine it.**

## 4. Dry Run
`arr = [2, 0, 1, 2, 1, 0]`

| Step | low | mid | high | arr[mid] | Action | Array state |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 5 | 2 | swap(mid,high), high-- | `[0,0,1,2,1,2]` |
| 1 | 0 | 0 | 4 | 0 | swap(low,mid), low++, mid++ | `[0,0,1,2,1,2]` |
| 2 | 1 | 1 | 4 | 0 | swap(low,mid), low++, mid++ | `[0,0,1,2,1,2]` |
| 3 | 2 | 2 | 4 | 1 | mid++ | `[0,0,1,2,1,2]` |
| 4 | 2 | 3 | 4 | 2 | swap(mid,high), high-- | `[0,0,1,1,2,2]` |
| 5 | 2 | 3 | 3 | 1 | mid++ | `[0,0,1,1,2,2]` |
| loop ends | | mid(4) > high(3) | | | | **`[0,0,1,1,2,2]`** ✓ |

## 5. Complexity
- Time: O(n) — single pass, each pointer moves monotonically, total work bounded by array length.
- Space: O(1) — in-place swaps only.

## 6. Edge Cases
- Already sorted array → algorithm still runs correctly, just makes fewer/no-op swaps.
- All same value (e.g. all 2s) → `high` decrements to `-1` immediately or `mid` sweeps straight to the end depending on the value; verify by tracing — algorithm handles uniformly, no special case needed.
- Array of size 0 or 1 → loop condition `mid <= high` naturally handles these (loop doesn't execute or executes trivially).

## 7. State the Invariant Out Loud in Interviews
This is a classic "prove your loop invariant" interview question. Being able to say — clearly, without prompting — *"at any point, everything before `low` is a finalized 0, between `low` and `mid` is a finalized 1, between `mid` and `high` inclusive is unknown, and after `high` is a finalized 2"* is what distinguishes "memorized the swaps" from "understands the algorithm." Expect a follow-up: *"why doesn't mid increment in the 2 case?"* — have the answer in Section 3 ready.

## 8. Related Patterns
- This is the **general 3-way partitioning template** — same shape solves "move all negatives to the front," "partition around a pivot value with three buckets," etc. Recognize this as reusable beyond literally 0/1/2.
- Distinct from the converging-pointer pattern in `11-two-sum.md` / `12-container-with-most-water.md` — this uses **same-direction** pointer movement (partitioning), not opposite-end convergence (pair-finding). Don't conflate the two sub-patterns of "two pointers."

## 9. NeetCode 150 Cross-Reference
- LeetCode 75 — Sort Colors (this lecture, exact match)

## 10. My Notes / Confusion Points

