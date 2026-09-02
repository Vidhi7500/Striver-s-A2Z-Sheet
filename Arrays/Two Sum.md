# Two Sum

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/two-sum/ (LeetCode 1)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given an array of integers `nums` and an integer `target`, return the indices of the two numbers that add up to `target`. Assume exactly one solution exists, and the same element can't be used twice.

**Example:**
```
Input:  nums = [2, 7, 11, 15], target = 9
Output: [0, 1]   (nums[0] + nums[1] = 2 + 7 = 9)
```

---

## Approach 1: Brute Force
**Idea:** Check every pair of indices with a nested loop until one sums to `target`.

- **Time Complexity:** O(n²)
- **Space Complexity:** O(1)

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) return {i, j};
        }
    }
    return {-1, -1};
}
```

---

## Approach 2: Hash Map, Single Pass (Optimal)
**Idea:** Walk through the array once. At each index `i`, check whether the **complement** (`target - nums[i]`) has already been seen. If it has, its stored index plus the current index `i` is the answer. If not, store the current value and its index for future lookups.

- **Time Complexity:** O(n) — single pass, O(1) average hash map lookups
- **Space Complexity:** O(n) — hash map can hold up to n entries

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        unordered_map<int, int> mp;
        for (int i = 0; i < n; i++) {
            if (mp.find(target - nums[i]) == mp.end()) {
                mp[nums[i]] = i;
            } else {
                return {i, mp[target - nums[i]]};
            }
        }
        return {-1, -1};
    }
};
```

**Walkthrough on `nums = [2, 7, 11, 15], target = 9`:**
```
i=0: complement = 9-2 = 7, not in map → mp = {2: 0}
i=1: complement = 9-7 = 2, found in map at index 0 → return {1, 0}
```

---

## Key Takeaways / Patterns
- **The "complement lookup" pattern** — checking `target - nums[i]` against a hash map — is one of the most reused array/string patterns in interviews (Two Sum variants, subarray sum problems, pair-difference problems).
- Checking-before-inserting in the same pass (rather than building the full map first, then scanning) is what gets this down to a **single pass** instead of two, and also correctly avoids using the same element twice.
- Returned order is `{i, mp[...]}` — i.e. current index first, then the earlier-stored index. Either order is typically accepted unless the problem specifies index ordering, but worth double-checking problem statement/judge expectations.

## Edge Cases Considered
- [ ] Negative numbers in the array
- [ ] Target achieved using the same value at two different indices (e.g. `nums=[3,3], target=6`)
- [ ] Solution pair at the very start vs very end of the array
- [ ] No valid pair exists (constraint says this won't happen, but worth knowing the fallback `{-1,-1}` exists)

## Related Problems
- Two Sum II (sorted array — two pointers instead of hashing)
- 3Sum / 4Sum (extensions of the same complement idea)
- Subarray Sum Equals K (prefix-sum + hash map, same "seen before" pattern)

## Mistakes I Made (if any)
- None — clean single-pass hash map solve on first try.
