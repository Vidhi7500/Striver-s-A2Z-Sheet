# Linear Search

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://takeuforward.org/data-structure/linear-search-in-c/  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given an array `arr` and a target value `x`, find the index of `x` in the array. Return `-1` if it doesn't exist.

**Example:**
```
Input:  arr = [3, 6, 1, 8, 4], x = 8
Output: 3

Input:  arr = [3, 6, 1, 8, 4], x = 10
Output: -1
```

---

## Approach: Linear Scan (Optimal for unsorted input)
**Idea:** Walk through the array one element at a time, comparing against `x`. Return the index the moment a match is found; if the loop finishes with no match, return `-1`.

- **Time Complexity:** O(n) — worst case scans the whole array
- **Space Complexity:** O(1)

```cpp
class Solution {
  public:
    int search(vector<int>& arr, int x) {
        int n = arr.size();
        for (int i = 0; i < n; i++) {
            if (arr[i] == x) {
                return i;
            }
        }
        return -1;
    }
};
```

---

## Key Takeaways / Patterns
- This is the baseline every other searching problem is compared against — O(n), no assumptions about the array (unsorted, duplicates, anything goes).
- The moment the array is guaranteed **sorted**, this should be immediately replaced by Binary Search (O(log n)) instead — worth flagging in review since it's an easy free upgrade to miss.
- Early return on match (instead of storing the index and breaking) keeps this clean and avoids an extra variable.

## Edge Cases Considered
- [ ] Target not present in array
- [ ] Target at index 0
- [ ] Target at last index
- [ ] Empty array
- [ ] Multiple occurrences of target (returns the *first* index found)

## Related Problems
- Binary Search (once array is sorted)
- Find First and Last Position of Element in Sorted Array
- Search in Rotated Sorted Array

## Mistakes I Made (if any)
- None — straightforward on first try.
