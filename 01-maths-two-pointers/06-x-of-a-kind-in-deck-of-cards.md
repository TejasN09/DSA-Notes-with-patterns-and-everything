# X of a Kind in a Deck of Cards

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** X of a kind of kind in a deck
**NeetCode 150 / LeetCode:** LeetCode 914 — "X of a Kind in a Deck of Cards" (exact same problem, appears in various curated lists alongside NeetCode 150-adjacent problem sets)
**Status:** ✅ Understood

---

## 1. Problem Statement
You have a deck of cards, each card an integer. Determine if you can split the deck into one or more groups such that:
- Each group has exactly `X` cards, where `X >= 2`
- All cards in each group have the same integer value

Return `true` if some valid `X` exists, `false` otherwise.

**Example:** `deck = [1,2,3,4,4,3,2,1]` → true (can split into 4 groups of 2 matching pairs: `[1,1],[2,2],[3,3],[4,4]`)
`deck = [1,1,1,2,2,2,3,3]` → true (X=1 doesn't count since X must be ≥2, but X could be... check: counts are {1:3, 2:3, 3:2} → gcd(3,3,2)=1 → false actually, since gcd must be ≥2)

## 2. Approach
**This is GCD-of-array in disguise — the actual skill is the reduction, not the algorithm.**

1. Count the frequency of each distinct card value (hashmap: value → count).
2. Compute the GCD of **all the frequencies** (not the card values themselves!).
3. Answer is `true` if and only if `gcd(frequencies) >= 2`.

**Why this works:** if you want to split into groups of exactly `X` matching cards, `X` must divide every value's frequency evenly (you can't split a group of 3 identical cards into groups of size 2 with none left over). The **largest** valid `X` is exactly the GCD of all the frequencies. If that GCD is `>= 2`, some valid grouping exists (use `X = gcd`). If the GCD is `1`, no `X >= 2` can divide all frequencies evenly — impossible.

## 3. Code
```java
static boolean hasGroupsSizeX(int[] deck) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int card : deck) {
        freq.put(card, freq.getOrDefault(card, 0) + 1);
    }

    int result = 0;
    for (int count : freq.values()) {
        result = gcd(result, count); // gcd(0, x) = x, clean start
        if (result == 1) return false; // early exit, can't recover
    }
    return result >= 2;
}
```

## 4. Dry Run
`deck = [1,2,3,4,4,3,2,1]`
- Frequencies: `{1:2, 2:2, 3:2, 4:2}`
- `gcd(2,2,2,2) = 2`
- `2 >= 2` → **true**

`deck = [1,1,1,2,2,2,3,3]`
- Frequencies: `{1:3, 2:3, 3:2}`
- `gcd(3,3,2)`: `gcd(3,3)=3`, `gcd(3,2)=1`
- `1 >= 2` is false → **false**

## 5. Complexity
- Time: O(n) to build frequency map + O(d log(max count)) to fold GCD, where `d` = number of distinct values. Overall O(n).
- Space: O(d) for the frequency map.

## 6. Edge Cases
- All cards identical → frequency is `[n]`, gcd is `n` itself → true if `n >= 2`.
- Every value appears exactly once → all frequencies are `1` → gcd is `1` → false (can't form groups of size ≥2).
- Deck size 1 → trivially false (can't form any group of size ≥2).

## 7. ⭐ Meta-Skill (the actual point of this problem)
The hard part isn't the GCD computation — it's recognizing that **"partition into equal-size matching groups" translates to "GCD of a derived frequency array."** This pattern — build a derived array (frequencies, differences, ratios) and apply a known technique to *that* instead of the raw input — is a recurring interview move. When a problem talks about "equal grouping," "equal distribution," or "divide evenly," check if frequencies + GCD solves it before reaching for anything more complex.

## 8. Related Patterns
- Direct application of `01-gcd-fundamentals.md` and `02-gcd-of-array.md`
- Same "derived array" instinct is useful in hashmap-heavy problems later in the course (Section 07)

## 9. My Notes / Confusion Points

