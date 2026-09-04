# Remove Duplicates from Sorted Array

**Topic:** Arrays
**Difficulty:** Easy
**Link:** https://leetcode.com/problems/remove-duplicates-from-sorted-array/
**Date solved:** 2026-09-04
**Status:** ✅ Solved

---

## Problem Statement

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates **in-place** such that each unique element appears only once.

Return the number of unique elements `k`.

The first `k` elements of the array should contain the unique elements in sorted order. The elements after index `k - 1` can be ignored.

**Example:**

```text
Input:  nums = [1, 1, 2]
Output: 2
Array after removal: [1, 2, _]

Input:  nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5
Array after removal: [0,1,2,3,4,_,_,_,_,_]
```

---

## Approach: Optimal (Single Pass / Two Pointers)

**Idea:** Since the array is already sorted, all duplicate elements will appear next to each other.

We maintain:

* `j` → scans the array to find the next unique element.
* `i` → points to the position where the next unique element should be placed.
* `ele` → stores the most recently placed unique element.

For every element:

1. If `nums[j] == ele`, it is a duplicate, so skip it.
2. Otherwise, we found a new unique element.
3. Update `ele`.
4. Store the unique element at `nums[i]`.
5. Increment `i`.
6. Finally, `i` represents the number of unique elements.

### Dry Run

For:

```text
nums = [1, 1, 2, 2, 3]
```

Initially:

```text
ele = 1
i = 1
```

| `j` | `nums[j]` | `ele` | Action                           | Array         |
| --: | --------: | ----: | -------------------------------- | ------------- |
|   1 |         1 |     1 | Duplicate → skip                 | `[1,1,2,2,3]` |
|   2 |         2 |     1 | New element → place at `nums[i]` | `[1,2,2,2,3]` |
|   3 |         2 |     2 | Duplicate → skip                 | `[1,2,2,2,3]` |
|   4 |         3 |     2 | New element → place at `nums[i]` | `[1,2,3,2,3]` |

At the end:

```text
i = 3
```

Therefore:

```text
k = 3
Unique elements = [1, 2, 3]
```

### Code

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();

        int ele = nums[0];
        int i = 1;

        for (int j = 1; j < n; j++) {
            if (nums[j] == ele) {
                continue;
            }
            else {
                ele = nums[j];
                nums[i] = nums[j];
                i++;
            }
        }

        return i;
    }
};
```

---

## Why This Works

The important property is that the input array is **sorted**.

For example:

```text
[1, 1, 1, 2, 2, 3, 3, 4]
```

All occurrences of the same number are consecutive.

Therefore, we only need to compare the current element with the **previous unique element**.

Whenever we find a different value:

```cpp
nums[j] != ele
```

we know it is a new unique element and move it to the next available position:

```cpp
nums[i] = nums[j];
```

This modifies the array **in-place**, so no additional array is required.

---

## Time & Space Complexity

* **Time Complexity:** `O(n)`

  * The array is traversed only once using `j`.

* **Space Complexity:** `O(1)`

  * Only a few variables (`n`, `ele`, `i`, `j`) are used.
  * No extra array or data structure is created.

---

## Key Takeaways / Patterns

* **Sorted array → duplicates are adjacent**, which allows us to solve the problem in a single pass.
* This is a classic **two-pointer / slow-fast pointer** pattern.
* `j` acts as the **fast pointer** that scans the array.
* `i` acts as the **slow pointer** that maintains the position for the next unique element.
* `ele` keeps track of the last unique element that was placed.
* Since the problem requires an **in-place** modification, we overwrite the beginning of the same array instead of creating another array.
* The returned value `i` represents the number of unique elements, not the last index.

---

## Edge Cases Considered

* [ ] Single element array → `[1]` → return `1`
* [ ] All elements are the same → `[2,2,2,2]` → return `1`
* [ ] No duplicates → `[1,2,3,4]` → return `4`
* [ ] Duplicates at the beginning → `[1,1,2,3]`
* [ ] Multiple consecutive duplicates → `[1,1,1,2,2,3]`
* [ ] Negative numbers → `[-3,-3,-2,-1,-1]`
* [ ] Mixed positive and negative values

> **Note:** LeetCode guarantees `nums.length >= 1`, so `nums[0]` is safe to access for this problem.

---

## Related Problems

* Remove Duplicates from Sorted Array II
* Remove Element
* Move Zeroes
* Check if Array is Sorted
* Merge Sorted Array

---

## Mistakes I Made (if any)

* No major logical mistake in the final solution.
* The important thing to remember is that this approach **depends on the array being sorted**. If the array were unsorted, simply comparing with the previous unique element would not be sufficient.
* The returned value is `i`, because `i` represents the **count of unique elements** and also the length of the valid portion of the modified array.
