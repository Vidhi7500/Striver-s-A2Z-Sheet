# Check if Array is Sorted

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://takeuforward.org/data-structure/check-if-an-array-is-sorted/  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given an array of integers, check whether the array is sorted in non-decreasing (ascending) order.

**Example:**
```
Input:  arr = [1, 2, 2, 3, 4]
Output: true

Input:  arr = [1, 3, 2, 4]
Output: false
```

---

## Approach 1: Brute Force
**Idea:** Create a sorted copy of the array, compare element-by-element with the original.

- **Time Complexity:** O(n log n)
- **Space Complexity:** O(n)

```cpp
bool isSorted(vector<int>& arr) {
    int n = arr.size();
    vector<int> num = arr;
    sort(num.begin(), num.end());
    for (int i = 0; i < n; i++) {
        if (num[i] != arr[i]) {
            return false;
        }
    }
    return true;
}
```

---

## Approach 2: Optimal (Single Pass)
**Idea:** Walk through the array once, comparing each element with the next. If any element is greater than its successor, it's not sorted.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
bool isSorted(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            return false;
        }
    }
    return true;
}
```

> **Note:** an earlier version of this had an extra trailing check `if (arr[n-1] < arr[0]) return false;`. Removed it — it's dead code here (if the loop passes, sorted order already guarantees `arr[n-1] >= arr[0]`), and it also crashes on an empty array via out-of-bounds access. That check belongs to the *"Check if Sorted and Rotated"* variant instead, where wrap-around actually matters.

---

## Key Takeaways / Patterns
- "Compare adjacent pairs" pattern — same idea reused in checking rotated sorted arrays, finding peak elements, etc.
- No need to build a whole sorted copy when you only care about a yes/no check — a single linear pass is always enough for this kind of validation problem.
- Loop condition `i < n - 1` (comparing `arr[i]` with `arr[i+1]`) is a common off-by-one trap — double check bounds.

## Edge Cases Considered
- [ ] Empty array (trivially sorted)
- [ ] Single element array (trivially sorted)
- [ ] Array with all equal elements (non-decreasing → sorted)
- [ ] Already sorted array
- [ ] Descending array
- [ ] Sorted-then-rotated array (should return false, e.g. [3,4,5,1,2])

## Related Problems
- Check if Array is Sorted and Rotated
- Largest Element in Array
- Second Largest Element in Array

## Mistakes I Made (if any)
- Added a redundant `arr[n-1] < arr[0]` wrap-around check (carried over from the "Sorted and Rotated" problem) — it's dead code for plain sorted-check and crashes on an empty array. Removed it.
