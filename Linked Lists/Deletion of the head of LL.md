# Delete Head of Linked List

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** _(add link here)_  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved 

---

## Problem Statement
Given the `head` of a linked list, delete the head node and return the new head.

**Example:**
```
Input:  head = [1,2,3]
Output: [2,3]
```

---

## Approach: Direct Deletion (submitted)
**Idea:** Save the current head in a temp pointer, move `head` forward by one, free the old head, and return the new head.

- **Time Complexity:** O(1)
- **Space Complexity:** O(1)

```cpp
class Solution {
  public:
    Node *deleteHead(Node *head) {
        Node* curr = head;
        head = head->next;
        delete curr;
        return head;
    }
};
```

**Walkthrough on `[1,2,3]`:**
```
curr = node 1
head = head->next = node 2
delete node 1
return head → [2, 3] ✅
```

---

## ⚠️ Edge Case Bug: Empty List
This crashes on an **empty list** (`head == NULL`) — `head->next` dereferences a null pointer, which is undefined behavior (segfault in most environments). Worth adding a guard at the top:

```cpp
Node *deleteHead(Node *head) {
    if (head == NULL) return NULL;   // nothing to delete
    Node* curr = head;
    head = head->next;
    delete curr;
    return head;
}
```

Most judges (GFG/LeetCode) don't test with a genuinely empty list for a "delete head" problem, which is likely why this passed — but it's the kind of edge case an interviewer will explicitly probe for, so worth fixing as a habit.

---

## Key Takeaways / Patterns
- **Always null-check `head` before dereferencing it** in any linked-list deletion function — this is the single most common source of crashes in linked-list problems.
- Deleting the head is O(1), same as inserting at the head — both just involve re-pointing/returning a reference, no traversal.
- `delete curr` (freeing memory) matters in C++ since there's no garbage collector — good habit to keep, even though many online judges won't penalize a memory leak if this line were omitted.

## Edge Cases Considered
- [x] Empty list — **currently crashes, needs the guard above**
- [ ] Single node list (after deletion, head becomes NULL — verify this works)
- [ ] Multiple nodes

## Related Problems
- Insert at Front of Linked List
- Delete at End of Linked List
- Delete a Given Node (without access to head)

## Mistakes I Made (if any)
- Missing a null check for the empty-list case before dereferencing `head->next` — added a guard clause to fix.
