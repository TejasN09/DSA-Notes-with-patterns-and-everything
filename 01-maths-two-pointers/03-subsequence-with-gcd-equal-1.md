# Check If a Subsequence Exists With GCD == 1

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Check if there exists a subsequence with gcd == 1
**NeetCode 150:** Not present — this exact framing is course-specific, but the underlying monotonicity insight is a recurring theme in subset/subsequence-GCD problems across LeetCode (e.g. "Smallest Subsequence with GCD 1" style problems).
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an array of `n` positive integers, determine whether **any subsequence** (not necessarily contiguous, not necessarily the whole array) has GCD exactly `1`.

## 2. Approach
**Key insight (this is the whole problem):** GCD of a set is monotonically non-increasing as you add more elements to it. So the GCD of the **entire array** is the *minimum possible* GCD achievable by any subset/subsequence of it — you cannot do better (lower) by picking a smaller subset.

Therefore: a subsequence with GCD 1 exists **if and only if** `gcd(all elements) == 1`.

**Proof sketch:** if the whole array's GCD is `g > 1`, then every element is divisible by `g`, so every subset's GCD is also divisible by `g` — no subset can ever reach GCD 1. If the whole array's GCD is exactly 1, the full array itself is a valid subsequence achieving GCD 1 — done.

## 3. Code
```java
static boolean subsequenceWithGcdOne(int[] arr) {
    int result = arr[0];
    for (int i = 1; i < arr.length; i++) {
        result = gcd(result, arr[i]);
        if (result == 1) return true;
    }
    return result == 1;
}
```
This is literally `gcdOfArray()` from the previous file with an early `true` return — same function, reframed as a decision problem.

## 4. Dry Run
`arr = [6, 10, 15]`
- `gcd(6, 10) = 2`
- `gcd(2, 15) = 1` → return **true**

Sanity check: is there a real subsequence achieving 1? `gcd(6, 10, 15) = 1` — yes, the whole array works. Also `gcd(10, 15) = 5`, `gcd(6, 15) = 3`, `gcd(6,10) = 2` — no *pair* works, but all three together does. This is exactly why you can't just check pairs; you need the full monotonic fold.

## 5. Complexity
- Time: O(n log(max element))
- Space: O(1)

## 6. Edge Cases
- Single element `[1]` → GCD is 1 trivially → true.
- Single element `[x]` where `x > 1` → GCD is `x` → false (no other elements to combine with).
- All elements share a common factor `> 1` → always false, no subsequence can escape that shared factor.

## 7. Interview Framing Tip
If the interviewer asks this, don't jump straight to code — **say the monotonicity insight out loud first**. This is a "does the candidate see the trick" question; the code is trivial once you see it, but explaining *why* checking the whole array suffices is what actually gets evaluated.

## 8. Related Patterns
- Direct extension of `02-gcd-of-array.md`
- Same monotonicity idea underlies `05-count-subarray-gcd-k.md`

## 9. My Notes / Confusion Points
