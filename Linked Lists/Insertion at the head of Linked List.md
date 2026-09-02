# Insert at Front of Linked List

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** https://www.geeksforgeeks.org/problems/linked-list-insertion-at-beginning/1  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved 

---

## Problem Statement
Given the `head` of a linked list and a value `x`, insert a new node with value `x` at the front of the list and return the new head.

**Example:**
```
Input:  head = [1,2,3], x = 0
Output: [0,1,2,3]
```

---

## Approach: Direct Insertion (Optimal)
**Idea:** Create a new node, point its `next` to the current `head`, and return the new node as the new head. No traversal needed since insertion is always at position 0.

- **Time Complexity:** O(1)
- **Space Complexity:** O(1) extra (excluding the new node itself)

```cpp
class Solution {
  public:
    Node *insertAtFront(Node *head, int x) {
        Node* curr = new Node(x);
        curr->next = head;
        return curr;
    }
};
```

**Walkthrough on `head = [1,2,3], x = 0`:**
```
curr = new Node(0)
curr->next = head  →  0 -> 1 -> 2 -> 3
return curr (new head is node 0)
```

---

## Key Takeaways / Patterns
- Inserting at the **front** is always O(1) for a singly linked list — no traversal required, unlike inserting at the end or at an arbitrary position, both of which need a pointer walk to find the insertion point.
- Always **return the new head** from insertion-at-front functions — the caller's reference to the original head is now stale, since the actual head has changed.
- This is the building block behind constructing a linked list from scratch by repeated front-insertions (though that reverses insertion order — insert-at-end is used instead when order matters).

## Edge Cases Considered
- [ ] Empty list (`head == NULL`) — should still work, new node's `next` becomes `NULL`
- [ ] Single node list
- [ ] Inserting at front repeatedly (building a list)

## Related Problems
- Insert at End of Linked List
- Insert at a Given Position
- Delete at Front of Linked List

## Mistakes I Made (if any)
- None — straightforward O(1) insertion on first try.
