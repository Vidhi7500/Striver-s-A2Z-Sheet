# Union of Two Sorted Arrays

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://www.geeksforgeeks.org/problems/union-of-two-sorted-arrays-1587115621/1  
**Date solved:** 2026-08-31  
**Status:** ✅ Solved

---

## Problem Statement
Given two **sorted** arrays `a` and `b`, return their union as a sorted array with no duplicates.

**Example:**
```
Input:  a = [1, 2, 3, 4, 5], b = [2, 3, 4, 4, 5]
Output: [1, 2, 3, 4, 5]
```

---

## Approach 1: Two-Pointer Merge + Hash Set (submitted)
**Idea:** Merge both arrays like in merge sort using two pointers, but insert every visited element into an `unordered_set` to auto-dedupe, then dump the set into a vector and sort it before returning.

- **Time Complexity:** O(n + m) for the merge/insert, **but O(k log k)** for the final sort (k = size of union) → net O((n+m) log(n+m))
- **Space Complexity:** O(n + m) — for both the set and the output vector

```cpp
class Solution {
  public:
    vector<int> findUnion(vector<int> &a, vector<int> &b) {
        int n = a.size();
        int m = b.size();
        int i = 0, j = 0;
        unordered_set<int> st;
        vector<int> v;
        while (i < n && j < m) {
            if (a[i] <= b[j]) {
                st.insert(a[i]);
                i++;
            } else {
                st.insert(b[j]);
                j++;
            }
        }
        while (i < n) {
            st.insert(a[i]);
            i++;
        }
        while (j < m) {
            st.insert(b[j]);
            j++;
        }
        for (auto it : st) {
            v.push_back(it);
        }
        sort(v.begin(), v.end());
        return v;
    }
};
```

**Why the final sort is wasted work:** the two-pointer merge already visits elements in sorted order — that's the whole point of merging two sorted arrays. Routing through an `unordered_set` throws that order away (it's unordered by definition), forcing a full re-sort at the end just to recover what was already there.

---

## Approach 2: Two-Pointer Merge, Skip Duplicates Directly (Optimal)
**Idea:** Same two-pointer merge, but instead of a set, only push a value onto the result if it's different from the *last value pushed*. Since input is sorted, equal elements are always adjacent in the merge — no hashing needed.

- **Time Complexity:** O(n + m) — single pass, no extra sort
- **Space Complexity:** O(n + m) — output vector only (no set)

```cpp
class Solution {
  public:
    vector<int> findUnion(vector<int> &a, vector<int> &b) {
        int n = a.size(), m = b.size();
        int i = 0, j = 0;
        vector<int> v;

        auto pushIfNew = [&](int val) {
            if (v.empty() || v.back() != val) v.push_back(val);
        };

        while (i < n && j < m) {
            if (a[i] <= b[j]) {
                pushIfNew(a[i]);
                i++;
            } else {
                pushIfNew(b[j]);
                j++;
            }
        }
        while (i < n) { pushIfNew(a[i]); i++; }
        while (j < m) { pushIfNew(b[j]); j++; }

        return v;
    }
};
```

---

## Key Takeaways / Patterns
- **Don't discard structure you already have.** Sorted input + a merge step already gives you sorted output for free — reaching for a hash set is a "brute force reflex" that erases that guarantee and costs you a sort at the end.
- Dedup-by-checking-last-pushed-element is the standard trick whenever duplicates can only be adjacent (true for sorted data) — no need for a set or map at all.
- This merge skeleton is identical to the merge step of merge sort — good to recognize as the same pattern.

## Edge Cases Considered
- [ ] One array is empty
- [ ] Both arrays fully overlap (identical arrays)
- [ ] No overlap at all
- [ ] Arrays with internal duplicates (e.g. `a = [2, 2, 3]`)
- [ ] Arrays of very different lengths

## Related Problems
- Intersection of Two Sorted Arrays
- Merge Two Sorted Arrays / Merge Sort merge step
- Remove Duplicates from Sorted Array

## Mistakes I Made (if any)
- Used `unordered_set` + final `sort()` when the two-pointer merge already guaranteed sorted order — redundant O(k log k) sort that a direct "skip if same as last" check would avoid entirely.
