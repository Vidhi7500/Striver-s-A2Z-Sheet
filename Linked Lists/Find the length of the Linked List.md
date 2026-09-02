# Count Nodes of Linked List

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** https://www.geeksforgeeks.org/problems/count-nodes-of-linked-list/1  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved 

---

## Problem Statement
Given the `head` of a linked list, count and return the total number of nodes in it.

**Example:**
```
Input:  head = [2, 3, 1, 9]
Output: 4
```

---

## Approach: Single Pass Traversal (Optimal)
**Idea:** Walk through the list from `head` to `NULL`, incrementing a counter at each node.

- **Time Complexity:** O(n) — must visit every node once
- **Space Complexity:** O(1)

```cpp
class Solution {
  public:
    int getCount(Node* head) {
        Node* curr = head;
        int cnt = 0;
        while (curr != NULL) {
            cnt++;
            curr = curr->next;
        }
        return cnt;
    }
};
```

**Walkthrough on `[2, 3, 1, 9]`:**
```
curr=2 → cnt=1
curr=3 → cnt=2
curr=1 → cnt=3
curr=9 → cnt=4
curr=NULL → loop ends
return 4 ✅
```

---

## Key Takeaways / Patterns
- Counting nodes is inherently O(n) — a singly linked list has no way to know its length without traversal (unlike an array's `.size()`), since there's no length field maintained by default.
- This "walk with a counter" traversal is the base skeleton reused constantly: finding the nth node from the end, finding the middle via count-then-jump, checking if length is odd/even, etc.
- Naturally handles the empty list case (`head == NULL`) correctly — loop never executes, returns 0.

## Edge Cases Considered
- [x] Empty list (`head == NULL`) — correctly returns 0
- [ ] Single node list
- [ ] Large list (just confirms O(n) is fine, no special handling needed)

## Related Problems
- Middle of the Linked List (brute-force approach uses this same counting idea)
- Nth Node from End of Linked List
- Delete Head of Linked List

## Mistakes I Made (if any)
- None — simple and correct on first try.
