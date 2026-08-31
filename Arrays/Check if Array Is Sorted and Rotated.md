# Check if Array Is Sorted and Rotated

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/ (LeetCode 1752)  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved / 🔁 Revisit / ❌ Stuck

---

## Problem Statement
Given an array `nums`, check if it was originally sorted in non-decreasing order, then possibly rotated some number of positions (including zero, i.e. it can still just be plain sorted).

**Example:**
```
Input:  nums = [3, 4, 5, 1, 2]
Output: true   (sorted array [1,2,3,4,5] rotated 3 times)

Input:  nums = [2, 1, 3, 4]
Output: false  (no rotation of a sorted array produces this)

Input:  nums = [1, 2, 3]
Output: true   (already sorted, i.e. rotated 0 times)
```

---

## Approach: Breakpoint Counting (Optimal)
**Idea:** In a sorted-and-rotated array, there can be **at most one point** where the "descending" pattern breaks (i.e. `nums[i] > nums[i+1]`) — that's the rotation point. Additionally, check the wrap-around pair `(nums[n-1], nums[0])` the same way, since the array is circular. If more than one such "break" exists, it can't be a rotated sorted array.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
class Solution {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        int cnt = 0;
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] > nums[i + 1]) cnt++;
        }
        if (nums[n - 1] > nums[0]) cnt++;
        if (cnt > 1) return false;
        return true;
    }
};
```

**Walkthrough on `[3, 4, 5, 1, 2]`:**
- `3<4` ok, `4<5` ok, `5>1` → cnt=1, `1<2` ok
- wrap-around: `nums[4]=2 > nums[0]=3`? No → cnt stays 1
- cnt = 1 ≤ 1 → `true` ✅

---

## Key Takeaways / Patterns
- **This is the missing piece from the plain "Check Sorted" problem** — the `nums[n-1] > nums[0]` wrap-around check that looked like dead code there is exactly what makes this rotated-array check work. Circular arrays almost always need this "close the loop" comparison between the last and first element.
- "Count breakpoints, allow at most 1" is a reusable pattern for any "was this rotated" style check.
- Note the direction of the comparison here (`nums[i] > nums[i+1]`) — same physical idea as the plain sorted-check, just repurposed to *count* violations instead of failing on the first one.

## Edge Cases Considered
- [ ] Already sorted, no rotation (`cnt` stays 0 → true)
- [ ] Single element array
- [ ] Array with duplicates, e.g. `[3,4,5,1,2,3]` — duplicates shouldn't create false breakpoints since check is strict `>`
- [ ] Rotated at every possible pivot point
- [ ] Two or more "breaks" → correctly returns false

## Related Problems
- Check if Array is Sorted (plain version — no rotation)
- Search in Rotated Sorted Array
- Find Minimum in Rotated Sorted Array

## Mistakes I Made (if any)
- None — clean on first try. Good reminder that the wrap-around check I removed from the plain "Check Sorted" notes wasn't wasted logic, it just belonged to *this* problem instead.
