# Majority Element

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/majority-element/ (LeetCode 169)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved 

---

## Problem Statement
Given an array `nums` of size `n`, return the majority element — the element that appears **more than `n/2` times**. Assumed to always exist in the given array.

**Example:**
```
Input:  nums = [2, 2, 1, 1, 1, 2, 2]
Output: 2
```

---

## Approach 1: Hash Map (submitted)
**Idea:** Count occurrences of every element in a map, then scan for the one whose count exceeds `n/2`.

- **Time Complexity:** O(n)
- **Space Complexity:** O(n) — hash map can hold up to n unique keys

```cpp
int majorityElement(vector<int>& nums) {
    int n = nums.size();
    unordered_map<int, int> mp;
    for (int i = 0; i < n; i++) {
        mp[nums[i]]++;
    }
    int ans = 0;
    for (auto it : mp) {
        if (it.second > n / 2) {
            ans = it.first;
        }
    }
    return ans;
}
```

---

## Approach 2: Boyer-Moore Voting Algorithm (Optimal — O(1) space)
**Idea:** Maintain a running `candidate` and a `count`. Walk through the array: if `count` is 0, adopt the current element as the new candidate. Increment `count` when the current element matches the candidate, decrement otherwise. Because the majority element appears more than n/2 times, it's mathematically guaranteed to "outlast" every other element canceling it out, so the candidate left standing at the end is the majority element.

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1) — no hash map needed

```cpp
int majorityElement(vector<int>& nums) {
    int count = 0;
    int candidate = 0;
    for (int x : nums) {
        if (count == 0) {
            candidate = x;
        }
        count += (x == candidate) ? 1 : -1;
    }
    return candidate;
}
```

**Walkthrough on `[2, 2, 1, 1, 1, 2, 2]`:**
```
x=2: count=0 → candidate=2, count=1
x=2: matches → count=2
x=1: no match → count=1
x=1: no match → count=0
x=1: count=0 → candidate=1, count=1
x=2: no match → count=0
x=2: count=0 → candidate=2, count=1
return candidate = 2 ✅
```

---

## Key Takeaways / Patterns
- **Boyer-Moore Voting** is the go-to O(1)-space trick whenever a problem guarantees an element appears **more than n/2 times** — this guarantee is exactly what makes the "cancel out" logic mathematically safe (a true majority element can never be fully cancelled by the rest of the array combined).
- Without the "more than n/2" guarantee, this algorithm can return a wrong candidate — it only *verifies* correctness if paired with a second pass counting the final candidate's actual occurrences. Worth knowing this caveat for the "Majority Element II" variant (n/3 threshold, up to 2 valid candidates).
- Hash map version is easy to reach for first, but interviewers specifically ask this problem to see if Boyer-Moore is known — good one to have memorized cold.

## Edge Cases Considered
- [ ] Single element array (trivially the majority element)
- [ ] Majority element at the very start vs scattered throughout
- [ ] All elements identical
- [ ] Majority element exactly at the n/2+1 threshold

## Related Problems
- Majority Element II (elements appearing more than n/3 times — extended voting algorithm)
- Single Number (same "cancel out via count" spirit, but using XOR instead)

## Mistakes I Made (if any)
- Reached for a hash map first — Boyer-Moore Voting is the expected O(1)-space answer for this exact problem shape and worth defaulting to once the "> n/2" guarantee is spotted.
