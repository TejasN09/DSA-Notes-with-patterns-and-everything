# Co-prime

**Course Section:** 02 — Maths and Two Pointers
**Lecture:** Co-prime
**NeetCode 150:** Not present as a standalone problem — coprimality checks appear as a sub-step inside other problems (fraction reduction, some tree/array coprime-pair counting problems on LeetCode).
**Status:** ✅ Understood

---

## 1. Definition
Two integers `a` and `b` are **co-prime** (relatively prime) if and only if `gcd(a, b) == 1` — they share no common factor other than 1.

**Important:** this is a **property check**, not a separate algorithm. Don't overthink it — it's a direct, one-line application of `01-gcd-fundamentals.md`.

```java
static boolean isCoprime(int a, int b) {
    return gcd(a, b) == 1;
}
```

## 2. Where This Shows Up in Practice
- **Counting coprime pairs in an array:** for each pair `(i, j)`, check `gcd(arr[i], arr[j]) == 1`. Brute force is O(n² log(max)) — often the *expected* answer unless `n` is very large, since there's no simpler general trick for arbitrary coprime-pair counting.
- **Fraction reduction:** a fraction `p/q` is in lowest terms iff `p` and `q` are coprime. Reduce by dividing both by `gcd(p, q)`.
- **Cycle detection in modular arithmetic problems:** step size and modulus being coprime often determines whether a sequence visits every residue (common in "jump around a circular array" problems).

## 3. Common Trap
When a problem says "co-prime," the instinct might be to reach for something more sophisticated (sieve, factorization). **Resist that** — coprimality is *just* `gcd == 1`. The complexity of a coprime-pair problem comes from the *counting/pairing structure* (how many pairs, over what range, with what additional constraints), not from computing GCD itself, which is O(log(max)) and trivial.

## 4. Worked Example — Counting Coprime Pairs
`arr = [2, 3, 4]`
- `gcd(2,3) = 1` → coprime ✓
- `gcd(2,4) = 2` → not coprime
- `gcd(3,4) = 1` → coprime ✓

Count of coprime pairs = **2**

```java
static int countCoprimePairs(int[] arr) {
    int n = arr.length, count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (gcd(arr[i], arr[j]) == 1) count++;
        }
    }
    return count;
}
```

## 5. Complexity
- Single check: O(log(min(a,b)))
- All-pairs counting: O(n² log(max element)) — the GCD call inside a double loop.

## 6. Edge Cases
- `gcd(1, x) = 1` always — 1 is coprime with every integer, including itself.
- `gcd(x, x) = x` — a number is only coprime with itself if that number is 1.
- `0` is technically coprime with nothing except 1 by some conventions (`gcd(0, x) = x`, so `gcd(0,x)=1` only if `x=1`) — clarify with interviewer if 0 can appear in input.

## 7. Related Patterns
- Pure application of `01-gcd-fundamentals.md` — no new algorithm here, just a naming convention worth memorizing so you don't hesitate when the word "coprime" appears in a problem statement.

## 8. My Notes / Confusion Points

