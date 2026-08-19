# Number of Subarrays With GCD == k

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Number of subarray with gcd == k
**NeetCode 150:** Not present — but the "reduce to a known sub-problem by dividing out k" trick generalizes to several LeetCode medium/hard problems involving divisibility constraints.
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an array of `n` positive integers and an integer `k`, count the number of **contiguous subarrays** whose GCD equals exactly `k`.

## 2. Approach

### Brute force — O(n²)
For each starting index `i`, extend `j` from `i` to `n-1`, maintaining a running GCD incrementally (`runningGcd = gcd(runningGcd, arr[j])` — don't recompute from scratch each time). Count whenever `runningGcd == k`.

```java
static int countSubarraysBruteForce(int[] arr, int k) {
    int n = arr.length, count = 0;
    for (int i = 0; i < n; i++) {
        int runningGcd = 0; // gcd(x, 0) = x, clean start
        for (int j = i; j < n; j++) {
            runningGcd = gcd(runningGcd, arr[j]);
            if (runningGcd == k) count++;
            // optional pruning: if runningGcd < k and k doesn't divide runningGcd... 
            // GCD only shrinks going forward, so once runningGcd < k, it can never equal k again for this i — break
            if (runningGcd < k) break;
        }
    }
    return count;
}
```
**Note the pruning:** since GCD is monotonically non-increasing as the subarray extends, once `runningGcd` drops below `k`, no further extension can bring it back up to `k` — safe to `break` early. This doesn't change worst-case complexity but helps average case a lot.

### Reduction trick (the real interview-level insight)
1. Every element must be **divisible by `k`** to possibly be part of a subarray with GCD `k` — any element not divisible by `k` breaks the subarray (splits it into segments).
2. Within a maximal segment of elements all divisible by `k`, divide every element by `k`.
3. Now count subarrays with GCD **exactly 1** in each segment — this is the *count* version of the "does a subsequence with GCD 1 exist" logic from `03-subsequence-with-gcd-equal-1.md`, extended from existence to counting (still requires the O(n²) contiguous-subarray scan within each segment, but on a smaller/cleaner problem).

**Why this reduction matters even though complexity doesn't improve:** it's the transferable skill. Recognizing "GCD == k" as "divide out k, now solve GCD == 1" is a move that shows up across multiple problem variants (divisibility-based counting, coprime-array problems) — interviewers care that you *see* the reduction, not just that you brute-force correctly.

## 3. Dry Run
`arr = [4, 8, 12, 6, 3]`, `k = 4`

Segments where element is divisible by 4: `[4, 8, 12]` (all divisible by 4), then `6` breaks it (6 not divisible by 4), then `3` isolated (also not divisible by 4).

Within `[4, 8, 12]`, divide by 4 → `[1, 2, 3]`. Count subarrays with GCD == 1:
- `[1]` → gcd 1 ✓
- `[1,2]` → gcd 1 ✓
- `[1,2,3]` → gcd 1 ✓
- `[2]` → gcd 2 ✗
- `[2,3]` → gcd 1 ✓
- `[3]` → gcd 3 ✗

Count = 4. These map back to original subarrays `[4]`, `[4,8]`, `[4,8,12]`, `[8,12]` — all have GCD 4 in the original array. Verify `gcd(8,12) = 4` ✓.

Answer: **4**

## 4. Complexity
- Brute force with incremental GCD: O(n²) worst case (still O(n²) even with pruning, but much faster in practice on typical data).
- No known O(n log n) general solution for this exact problem — O(n²) is the expected answer in an interview unless constraints are small.

## 5. Edge Cases
- `k` doesn't divide any element → answer is 0 immediately (can check this as a fast pre-pass).
- `k == 1` → this reduces to counting all subarrays with GCD exactly 1 directly, no division-by-k reduction needed.
- Single-element subarrays where `arr[i] == k` always count (a single element's "GCD" is itself).

## 6. Related Patterns
- Extends `02-gcd-of-array.md` (running GCD) and `03-subsequence-with-gcd-equal-1.md` (the reduction target)
- Monotonic-GCD pruning idea shows up again conceptually in monotonic stack problems (Section 08) — different mechanism, same "exploit monotonicity to prune" spirit.

## 7. My Notes / Confusion Points

