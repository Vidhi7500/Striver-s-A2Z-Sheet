# Move Zeroes

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/move-zeroes/ (LeetCode 283)  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given an array `nums`, move all `0`s to the end while maintaining the relative order of the non-zero elements. Must be done in-place without making a copy of the array.

**Example:**
```
Input:  nums = [0, 1, 0, 3, 12]
Output: [1, 3, 12, 0, 0]
```

---

## Approach: Two Pointers (Optimal)
**Idea:** `i` marks the position where the next non-zero element should go; `j` scans the array. Whenever `nums[j]` is non-zero, swap it into position `i` and advance `i`. `j` always advances. This pushes all non-zero elements to the front in order, and zeroes naturally end up shifted to the back.

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1) — in-place, only pointers used

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int i = 0, j = 0;
        while (j < n) {
            if (nums[j] != 0) {
                swap(nums[i], nums[j]);
                i++;
            }
            j++;
        }
    }
};
```

**Walkthrough on `[0, 1, 0, 3, 12]`:**
```
i=0 j=0: nums[0]=0 → skip, j++
i=0 j=1: nums[1]=1 → swap(0,1) → [1,0,0,3,12], i=1, j=2
i=1 j=2: nums[2]=0 → skip, j++
i=1 j=3: nums[3]=3 → swap(1,3) → [1,3,0,0,12], i=2, j=4
i=2 j=4: nums[4]=12 → swap(2,4) → [1,3,12,0,0], i=3, j=5
loop ends → [1, 3, 12, 0, 0] ✅
```

---

## Key Takeaways / Patterns
- Classic **"slow-fast pointer / partition"** pattern — same core idea as partitioning in QuickSort, or problems like "Remove Element" / "Segregate 0s and 1s" (Dutch National Flag, simplified to 2 values).
- `i` only advances when a "keep" condition is met; `j` always advances — this pattern generalizes to "move all elements satisfying X to the front/back in-place."
- Swapping (instead of overwriting) is what keeps this correct without needing a separate pass to fill in trailing zeroes.

## Edge Cases Considered
- [ ] Array with no zeroes (should remain unchanged)
- [ ] Array that is all zeroes
- [ ] Single element array
- [ ] Zeroes already at the end
- [ ] Zeroes already at the start

## Related Problems
- Remove Element (LeetCode 27)
- Sort Colors / Dutch National Flag (LeetCode 75)
- Segregate Even and Odd Numbers

## Mistakes I Made (if any)
- None — clean two-pointer solution on first try.
