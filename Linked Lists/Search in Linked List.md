# Search in Linked List

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** https://www.geeksforgeeks.org/problems/search-in-linked-list-1664434326/1  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given the `head` of a linked list and an integer `key`, check whether `key` exists in the list.

**Example:**
```
Input:  head = [2, 8, 3, 5, 9], key = 3
Output: true

Input:  head = [2, 8, 3, 5, 9], key = 7
Output: false
```

---

## Approach: Single Pass Traversal (Optimal)
**Idea:** Walk through the list node by node, comparing each node's `data` to `key`. Return `true` the moment a match is found; if the traversal finishes with no match, return `false`.

- **Time Complexity:** O(n) — worst case scans the whole list
- **Space Complexity:** O(1)

```cpp
class Solution {
  public:
    bool searchKey(Node* head, int key) {
        Node* curr = head;
        while (curr != NULL) {
            if (curr->data == key) {
                return true;
            }
            curr = curr->next;
        }
        return false;
    }
};
```

---

## Key Takeaways / Patterns
- This is the **linked-list equivalent of Linear Search on an array** — same idea, same O(n) complexity, just walking via `next` pointers instead of an index. Good to connect the two in review: unlike arrays, a linked list has no random access, so there's no faster alternative (no binary search possible without extra structure, even if the list were sorted).
- Early return on match keeps this clean, same pattern as the array version.
- Naturally handles the empty list case — loop never executes, correctly returns `false`.

## Edge Cases Considered
- [x] Empty list (`head == NULL`) — correctly returns `false`
- [ ] Key at the head
- [ ] Key at the tail
- [ ] Key not present
- [ ] Duplicate values of key in the list

## Related Problems
- Linear Search (array version)
- Count Nodes of Linked List (same traversal skeleton)
- Delete a Node by Value in Linked List (search + delete combined)

## Mistakes I Made (if any)
- None — clean traversal-based search on first try.
