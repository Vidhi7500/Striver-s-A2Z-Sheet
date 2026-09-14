# Longest Subarray with Sum K

**Topic:** Arrays  
**Difficulty:** Medium  
**Link:** https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given an array `arr` (which may contain negative numbers) and an integer `k`, find the length of the longest contiguous subarray whose sum equals `k`.

**Example:**
```
Input:  arr = [10, 5, 2, 7, 1, 9], k = 15
Output: 4   (subarray [5, 2, 7, 1] sums to 15)
```

---

## Approach 1: Brute Force
**Idea:** Check every subarray's sum and track the maximum length among those equal to `k`.

- **Time Complexity:** O(n²)
- **Space Complexity:** O(1)

```cpp
int longestSubarray(vector<int>& arr, int k) {
    int n = arr.size();
    int max_len = 0;
    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == k) max_len = max(max_len, j - i + 1);
        }
    }
    return max_len;
}
```

---

## Approach 2: Prefix Sum + Hash Map (Optimal — works with negatives)
**Idea:** Keep a running prefix sum. For the subarray ending at index `i` to sum to `k`, there must be some earlier prefix sum equal to `sum - k` — if the current prefix sum minus `k` was seen before at index `j`, then the subarray `(j+1 .. i)` sums to exactly `k`. Store the **first** occurrence of each prefix sum (using `insert`, not `[]`, so it never gets overwritten) to guarantee the longest possible subarray whenever that sum recurs.

- **Time Complexity:** O(n) — single pass, O(1) average hash map operations
- **Space Complexity:** O(n) — hash map of prefix sums

```cpp
class Solution {
  public:
    int longestSubarray(vector<int>& arr, int k) {
        int n = arr.size();
        int max_len = 0;
        int sum = 0;
        unordered_map<int, int> mp;

        for (int i = 0; i < n; i++) {
            sum += arr[i];
            if (sum == k) {
                max_len = max(max_len, i + 1);
            }
            if (mp.find(sum - k) != mp.end()) {
                max_len = max(max_len, i - mp[sum - k]);
            }
            mp.insert({sum, i});
        }
        return max_len;
    }
};
```

**Walkthrough on `[10, 5, 2, 7, 1, 9], k = 15`:**
```
i=0: sum=10,  no match, mp={10:0}
i=1: sum=15,  sum==k → max_len=2, mp={10:0, 15:1}
i=2: sum=17,  sum-k=2 not found, mp+={17:2}
i=3: sum=24,  sum-k=9 not found, mp+={24:3}
i=4: sum=25,  sum-k=10 found at index 0 → max_len=max(2, 4-0)=4, mp+={25:4}
i=5: sum=34,  sum-k=19 not found
return 4 ✅
```

---

## Key Takeaways / Patterns
- **Using `mp.insert()` instead of `mp[sum] = i` is the critical correctness detail here.** `insert` silently does nothing if the key already exists, which preserves the *first* (earliest) index for each prefix sum. Since a longer subarray always comes from subtracting against the earliest possible starting point, overwriting with a later index (as `[]` would do) could silently produce a shorter, wrong answer whenever a prefix sum repeats.
- This prefix-sum + hash map approach is the general solution and **works even with negative numbers**, unlike a sliding-window approach, which only works when all elements are non-negative (since window sums must monotonically grow as the window expands, which fails once negatives are allowed).
- Same "seen-before via hash map" family as Two Sum — checking `sum - k` against previously stored prefix sums mirrors checking `target - nums[i]` against previously stored values.

## Edge Cases Considered
- [ ] Array contains negative numbers (prefix sum can repeat — this is exactly why `insert` over `[]` matters)
- [ ] Entire array sums to `k` (handled by the `sum == k` check using `i+1`)
- [ ] No subarray sums to `k` (returns 0)
- [ ] Single element equal to `k`

## Related Problems
- Two Sum (same "complement via hash map" family)
- Subarray Sum Equals K (LeetCode 560 — count subarrays instead of longest length)
- Longest Subarray with Sum K (non-negative only) via Sliding Window — a simpler O(n), O(1)-space alternative when negatives are excluded

## Mistakes I Made (if any)
- None — correctly used `insert()` rather than `[]` to preserve the earliest prefix-sum index, which is the detail most likely to be gotten wrong in this problem.
