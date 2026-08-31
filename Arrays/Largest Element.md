# Largest Element in Array

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://takeuforward.org/data-structure/find-the-largest-element-in-an-array/  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given an array of integers, find the largest element in the array.

**Example:**
```
Input:  arr = [2, 5, 1, 3, 0]
Output: 5
```

---

## Approach 1: Brute Force
**Idea:** Sort the array, return the last element.

- **Time Complexity:** O(n log n)
- **Space Complexity:** O(1) or O(n) depending on sort implementation

```cpp
int largestElement(vector<int>& arr) {
    sort(arr.begin(), arr.end());
    return arr[arr.size() - 1];
}
```

---

## Approach 2: Optimal
**Idea:** Single pass, track the max seen so far.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
    int largest(vector<int> &arr) {
        // code here
        int n=arr.size();
        int max_num=arr[0];
        for(int i=1;i<n;i++){
            if(max_num<arr[i]){
                max_num=arr[i];
            }
        }
        return max_num;
    }
```

---

## Key Takeaways / Patterns
- Classic "single pass tracking" pattern — reusable for min, second-largest, max subarray sum, etc.
- Always ask: can I avoid sorting and do it in one linear scan?

## Edge Cases Considered
- [ ] Empty array
- [ ] Single element array
- [ ] All elements same
- [ ] Negative numbers

## Related Problems
- Second Largest Element in Array
- Kth Largest Element
- Maximum Subarray Sum (Kadane's)
