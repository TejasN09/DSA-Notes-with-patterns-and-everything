# Container With Most Water

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Container with most water
**NeetCode 150:** LeetCode 11 — "Container With Most Water" (exact match, core NeetCode 150 problem, frequently asked)
**Status:** ✅ Understood

---

## 1. Problem Statement
Given an array `height[]` where each element represents the height of a vertical line at that index, find two lines that, together with the x-axis, form a container holding the maximum amount of water. Return the max area.

`area = min(height[left], height[right]) * (right - left)`

## 2. When to Spot It (signal phrases)
- "maximize area/volume between two elements"
- Array of heights/values, pick two indices, area/capacity depends on the smaller of the two plus the distance between them.

## 3. Approach
Two pointers at opposite ends, converge inward.
```java
int left = 0, right = arr.length - 1, maxArea = 0;
while (left < right) {
    int height = Math.min(arr[left], arr[right]);
    maxArea = Math.max(maxArea, height * (right - left));
    if (arr[left] < arr[right]) left++;
    else right--;
}
```

**Why move the *shorter* line (this is the actual interview question, not just "what's the code"):**
- Width only shrinks as the pointers converge — it can never increase again.
- So the *only* way area can increase after this step is if height increases.
- Current area is capped by `min(arr[left], arr[right])` — the **shorter** of the two lines.
- If you move the **taller** line inward instead: the new pair's height is still `min(new_line, the_other_still-shorter_line)`, which is capped by the same shorter line as before — but now width is strictly smaller. Area cannot improve. You've wasted a step.
- If you move the **shorter** line: you at least give yourself a *chance* at a taller line, which could increase the height-cap and possibly the area, despite the smaller width.
- **This is a greedy-elimination argument**, not an exhaustive proof that the new area is bigger — it's a proof that moving the taller line can *never* be optimal, so moving the shorter line never loses a better answer.

## 4. Dry Run
`height = [1,8,6,2,5,4,8,3,7]`
- `left=0(1), right=8(7)`: area = `min(1,7)*8 = 8`. `1 < 7` → `left++`
- `left=1(8), right=8(7)`: area = `min(8,7)*7 = 49`. `8 > 7` → `right--`
- `left=1(8), right=7(3)`: area = `min(8,3)*6 = 18`. `8 > 3` → `right--`
- `left=1(8), right=6(8)`: area = `min(8,8)*5 = 40`. equal → move either, say `right--`
- ... continues, but **max found so far is 49** and remaining steps don't beat it.

Answer: **49**

## 5. Complexity
- Time: O(n) — each pointer moves at most `n` times total, single pass.
- Space: O(1)

## 6. Edge Cases
- All heights equal → area is maximized by using the two outermost indices (widest possible width, height is the constant value).
- Array of size 2 → only one possible pair, return that area directly (loop still handles this correctly, runs once).
- Heights containing 0 → valid, just means that line contributes zero holding capacity if chosen — algorithm handles naturally, no special case needed.

## 7. Common Mistake
Trying to solve this by checking all O(n²) pairs "just to be safe" even after recognizing the two-pointer pattern — if you can articulate the greedy-elimination proof above, you should trust it and go straight to O(n). Brute force here signals you haven't internalized *why* the greedy step is safe, which is exactly what this lecture is testing.

## 8. Related Patterns
- Same converging-pointer mechanism as `11-two-sum.md`
- Conceptually related to **Trapping Rain Water** (LeetCode 42, covered later in Arrays section) — that problem extends this idea using running max-from-left/right arrays instead of just two pointers, because it needs to account for *every* position's trapped water, not just one container.

## 9. NeetCode 150 Cross-Reference
- LeetCode 11 — Container With Most Water (this lecture, exact match — high priority, comes up often)
- LeetCode 42 — Trapping Rain Water (related but distinct — don't confuse the two approaches; flag for when you reach Arrays section)

## 10. My Notes / Confusion Points

