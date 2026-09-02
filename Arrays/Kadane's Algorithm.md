# Maximum Subarray

**Topic:** Arrays  
**Difficulty:** Medium  
**Link:** https://leetcode.com/problems/maximum-subarray/ (LeetCode 53)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given an integer array `nums`, find the contiguous subarray (containing at least one number) with the largest sum, and return that sum.

**Example:**
```
Input:  nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6   (subarray [4,-1,2,1] has the largest sum)
```

---

## Approach 1: Brute Force
**Idea:** Check every possible subarray's sum and track the maximum.

- **Time Complexity:** O(n²) — or O(n³) with a naive nested-sum recomputation
- **Space Complexity:** O(1)

```cpp
int maxSubArray(vector<int>& nums) {
    int n = nums.size();
    int max_sum = INT_MIN;
    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += nums[j];
            max_sum = max(max_sum, sum);
        }
    }
    return max_sum;
}
```

---

## Approach 2: Kadane's Algorithm (Optimal)
**Idea:** Track a running `sum` while scanning left to right. Keep adding elements; update `max_sum` at every step. The key insight: if the running sum ever drops below `0`, it can only hurt any future subarray sum, so reset it to `0` — effectively "starting fresh" from the next element rather than dragging along a negative prefix.

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1)

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int sum = 0;
        int max_sum = INT_MIN;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            max_sum = max(max_sum, sum);
            if (sum < 0) sum = 0;
        }
        return max_sum;
    }
};
```

**Walkthrough on `[-2,1,-3,4,-1,2,1,-5,4]`:**
```
i=0: sum=-2, max_sum=-2, sum<0 → reset sum=0
i=1: sum=1,  max_sum=1
i=2: sum=-2, max_sum=1, sum<0 → reset sum=0
i=3: sum=4,  max_sum=4
i=4: sum=3,  max_sum=4
i=5: sum=5,  max_sum=5
i=6: sum=6,  max_sum=6
i=7: sum=1,  max_sum=6
i=8: sum=5,  max_sum=6
return 6 ✅
```

---

## Key Takeaways / Patterns
- **`max_sum` is updated every iteration, before the reset check** — this ordering matters. Checking `max_sum` first captures the running sum at its peak *before* deciding whether to discard it, so even a subarray of length 1 (a single very negative number, if it's the only option) is correctly considered.
- Initializing `max_sum` to `INT_MIN` (not `0`) is essential for correctness when **all elements are negative** — the answer must still be the least-negative single element, not `0` (which isn't a valid subarray sum unless `0` genuinely appears).
- This is the canonical **local vs global optimum** pattern: `sum` tracks the best subarray *ending here*, `max_sum` tracks the best seen *anywhere so far*. This structure reappears in many "best subarray/subsequence" DP-flavored problems.

## Edge Cases Considered
- [x] All negative numbers — `INT_MIN` init handles this correctly (verify: should return the single largest negative number, not 0)
- [ ] All positive numbers (entire array is the answer)
- [ ] Single element array
- [ ] Mix of positive and negative with the best subarray in the middle

## Related Problems
- Maximum Product Subarray (same DP spirit, trickier due to negative*negative flips)
- Best Time to Buy and Sell Stock (running min/max tracking, same "local vs global" shape)
- Longest Subarray with Sum K

## Mistakes I Made (if any)
- None — correctly initialized `max_sum` to `INT_MIN` and ordered the update-before-reset logic correctly on first try.
