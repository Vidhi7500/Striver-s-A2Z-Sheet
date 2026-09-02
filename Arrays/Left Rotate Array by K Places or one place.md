# Rotate Array

**Topic:** Arrays  
**Difficulty:** Medium  
**Link:** https://leetcode.com/problems/rotate-array/ (LeetCode 189)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved 

---

## Problem Statement
Given an array `nums`, rotate the array to the right by `k` steps, in-place.

**Example:**
```
Input:  nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
```

---

## Approach: Three Reversals (Optimal)
**Idea:** Right-rotating by `k` is equivalent to: reverse the whole array, then reverse the first `k` elements, then reverse the remaining `n-k` elements. Reversing the whole array flips everything into the right relative order but backwards within each of the two segments; re-reversing each segment individually fixes their internal order back to normal while keeping the overall rotation.

`k` is first taken modulo `n` to handle cases where `k >= n` (rotating by a full array length or more is a no-op beyond `k % n`).

- **Time Complexity:** O(n) — three passes, each touching disjoint or full ranges, still linear overall
- **Space Complexity:** O(1) — in-place, only reversal operations

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        reverse(nums.begin(), nums.end());
        reverse(nums.begin() + k, nums.end());
        reverse(nums.begin(), nums.begin() + k);
    }
};
```

**Walkthrough on `[1,2,3,4,5,6,7], k=3`:**
```
reverse whole:            [7,6,5,4,3,2,1]
reverse index[3..6]:      [7,6,5,1,2,3,4]
reverse index[0..2]:      [5,6,7,1,2,3,4] ✅
```

---

## Key Takeaways / Patterns
- **`k = k % n` first** is essential — without it, `k > n` would make `nums.begin() + k` point past `end()`, causing undefined behavior. Always normalize a rotation count against the array length before using it as an index/offset.
- The order of the two segment reversals (first-k vs remaining-n-k) doesn't matter since they operate on completely disjoint ranges — either order gives the same final result.
- This three-reversal trick is a reusable pattern any time "rotate in place, O(1) space" comes up — much cleaner than a brute-force approach using an extra array or repeated single-step rotations (which would be O(n·k)).

## Edge Cases Considered
- [ ] `k == 0` (no rotation, all three reverses effectively cancel out)
- [ ] `k == n` (full rotation → no-op after `k % n` gives 0)
- [ ] `k > n` (handled by modulo)
- [ ] Single element array
- [ ] Array with duplicate values

## Related Problems
- Reverse Linked List (same "reversal" building block, different data structure)
- Rotate Image / Matrix
- Left Rotate Array by One / by D places (Striver sheet variants of this same idea)

## Mistakes I Made (if any)
- None — correctly applied `k % n` and the three-reversal trick on first try.
