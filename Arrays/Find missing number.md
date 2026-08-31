# Missing Number in Array

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://www.geeksforgeeks.org/problems/missing-number-in-array1416/1  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given an array of size `n-1` containing distinct integers from `1` to `n`, find the one missing number.

**Example:**
```
Input:  arr = [1, 2, 4, 5], n = 5
Output: 3
```

---

## Approach 1: Sort + Linear Scan (submitted)
**Idea:** Sort the array. In a complete 1..n sequence, `arr[i]` should equal `i+1`. The first index where this breaks is the missing number. If the loop completes with no mismatch, the missing number must be the last one, `n+1`.

- **Time Complexity:** O(n log n) — dominated by the sort
- **Space Complexity:** O(1) extra (in-place sort) / O(log n) sort stack space

```cpp
int missingNum(vector<int>& arr) {
    sort(arr.begin(), arr.end());
    int n = arr.size();
    for (int i = 0; i < n; i++) {
        if (arr[i] != i + 1) {
            return i + 1;
        }
    }
    return n + 1;
}
```

**Walkthrough on `arr = [1, 2, 4, 5]` (n=4, actual range is 1..5):**
```
i=0: arr[0]=1, expected 1 → ok
i=1: arr[1]=2, expected 2 → ok
i=2: arr[2]=4, expected 3 → mismatch! return 3 ✅
```

---

## Approach 2: Sum Formula (Optimal — no sort needed)
**Idea:** The expected sum of `1..n` is `n*(n+1)/2`. Subtract the actual sum of the array from this; the difference is the missing number. Avoids sorting entirely.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
int missingNum(vector<int>& arr) {
    int n = arr.size() + 1; // full range is 1..n
    int expectedSum = n * (n + 1) / 2;
    int actualSum = 0;
    for (int x : arr) actualSum += x;
    return expectedSum - actualSum;
}
```

---

## Approach 3: XOR (Optimal — avoids overflow risk on large sums)
**Idea:** XOR-ing all numbers `1..n` together with all array elements cancels out every pair that appears twice, leaving only the missing number.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
int missingNum(vector<int>& arr) {
    int n = arr.size() + 1;
    int xorAll = 0;
    for (int i = 1; i <= n; i++) xorAll ^= i;
    for (int x : arr) xorAll ^= x;
    return xorAll;
}
```

---

## Key Takeaways / Patterns
- Sorting works but is never truly optimal when a direct **math/bit trick** exists — always worth pausing on "1 to n" style problems to check for a sum or XOR shortcut.
- Sum formula is intuitive but can overflow for very large `n`; XOR avoids that entirely, which is why it's often preferred in interviews.
- This "expected vs actual" comparison (via sum or index-matching) is a recurring idea across missing/duplicate number problems.

## Edge Cases Considered
- [ ] Missing number is 1 (first element)
- [ ] Missing number is n (last element)
- [ ] Array of size 1 (single missing number to find from range 1..2)
- [ ] Large n (overflow risk with sum formula — XOR is safer)

## Related Problems
- Find the Duplicate Number
- Find All Numbers Disappeared in an Array
- Single Number (XOR pattern)

## Mistakes I Made (if any)
- None — clean solve, but worth revisiting to practice the XOR approach since sort was the first instinct.
