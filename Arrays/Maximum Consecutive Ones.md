# Max Consecutive Ones

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/max-consecutive-ones/ (LeetCode 485)  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given a binary array `nums`, return the maximum number of consecutive `1`s in the array.

**Example:**
```
Input:  nums = [1, 1, 0, 1, 1, 1]
Output: 3
```

---

## Approach: Single Pass Counter (Optimal)
**Idea:** Keep a running counter `cnt` of the current streak of 1s. On a `1`, increment it. On a `0`, the streak is broken — update `max_ones` and reset `cnt` to 0. After the loop, do one final update, since the longest streak might end at the very last element (no trailing `0` to trigger the update inside the loop).

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1)

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int max_ones = 0;
        int n = nums.size();
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 1) {
                cnt++;
            } else {
                max_ones = max(max_ones, cnt);
                cnt = 0;
            }
        }
        max_ones = max(max_ones, cnt);
        return max_ones;
    }
};
```

**Walkthrough on `[1, 1, 0, 1, 1, 1]`:**
```
i=0: 1 → cnt=1
i=1: 1 → cnt=2
i=2: 0 → max_ones=max(0,2)=2, cnt=0
i=3: 1 → cnt=1
i=4: 1 → cnt=2
i=5: 1 → cnt=3
loop ends → max_ones=max(2,3)=3 ✅
```

---

## Key Takeaways / Patterns
- The **final `max(max_ones, cnt)` after the loop** is the easy-to-forget part of this pattern — without it, an array ending in a streak of 1s (like the example above) would silently return the wrong answer. Good habit: whenever a "reset on break" pattern is used, check whether the streak needs a final flush after the loop.
- This is the simplest form of a much bigger family — the same "running counter + reset on break" idea scales up to Sliding Window problems (e.g. "Max Consecutive Ones III" allows flipping up to k zeros).

## Edge Cases Considered
- [ ] All ones (no zero to trigger reset — relies on final flush)
- [ ] All zeros (answer is 0)
- [ ] Single element array
- [ ] Streak at the very start followed by all zeros
- [ ] Alternating 1s and 0s

## Related Problems
- Max Consecutive Ones II / III (Sliding Window variants)
- Longest Subarray with Sum K
- Longest Substring Without Repeating Characters (same "window reset" family)

## Mistakes I Made (if any)
- None — remembered the final flush after the loop, which is the most common bug in this pattern.
