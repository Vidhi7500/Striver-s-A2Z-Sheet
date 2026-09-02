# Single Number

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/single-number/ (LeetCode 136)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given a non-empty array of integers where every element appears **twice** except for one, find that single element. Must run in linear time and use only constant extra space (per the problem's constraints).

**Example:**
```
Input:  nums = [4, 1, 2, 1, 2]
Output: 4
```

---

## Approach 1: Hash Map (submitted)
**Idea:** Count occurrences of every number in a map, then scan the map for the entry with count `1`.

- **Time Complexity:** O(n)
- **Space Complexity:** O(n) — hash map stores up to n/2 + 1 unique keys

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int, int> mp;
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            mp[nums[i]]++;
        }
        for (auto it : mp) {
            if (it.second == 1) {
                ans = it.first;
            }
        }
        return ans;
    }
};
```

---

## Approach 2: XOR (Optimal — meets the O(1) space constraint)
**Idea:** XOR of a number with itself is `0`, and XOR with `0` leaves a number unchanged. XOR-ing every element together cancels out all the pairs, leaving only the number that appears once.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1) — no extra data structure

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int x : nums) {
            ans ^= x;
        }
        return ans;
    }
};
```

**Walkthrough on `[4, 1, 2, 1, 2]`:**
```
ans = 0
ans ^= 4 → 4
ans ^= 1 → 5
ans ^= 2 → 7
ans ^= 1 → 6
ans ^= 2 → 4
return 4 ✅
```

---

## Key Takeaways / Patterns
- **Same XOR family as "Missing Number in Array"** — both rely on the self-cancelling property of XOR (`a ^ a = 0`, `a ^ 0 = a`) to eliminate the need for a hash structure. Worth reviewing both together as one pattern: "XOR cancels pairs/duplicates."
- The problem's own constraint (O(1) space) is a strong signal the intended solution is XOR, not hashing — good habit to check stated constraints before picking an approach, since they often point directly at the expected technique.
- The hash map version is correct but doesn't meet the problem's own space requirement — worth noting in an interview: "this works, but here's the O(1) space version the constraints are hinting at."

## Edge Cases Considered
- [ ] Array of size 1 (the single element itself)
- [ ] Single element at the start vs end of the array
- [ ] Negative numbers (XOR handles these fine via two's complement)
- [ ] Large arrays (XOR avoids any hashing overhead entirely)

## Related Problems
- Missing Number in Array (same XOR self-cancelling trick)
- Single Number II (element appears 3x except one — needs bit-counting, XOR alone isn't enough)
- Single Number III (two unique elements — XOR + bit partitioning)

## Mistakes I Made (if any)
- Reached for a hash map first even though the problem explicitly asks for O(1) space — XOR was the intended approach. Worth flagging constraints before coding next time.
