# Middle of the Linked List

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/middle-of-the-linked-list/ (LeetCode 876)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given the `head` of a singly linked list, return the middle node. If there are two middle nodes (even length), return the **second** middle node.

**Example:**
```
Input:  head = [1,2,3,4,5]
Output: node with val = 3

Input:  head = [1,2,3,4,5,6]
Output: node with val = 4  (second of the two middle nodes)
```

---

## Approach 1: Brute Force (Count + Traverse)
**Idea:** Traverse once to count total nodes `n`, then traverse again to the `n/2`-th node.

- **Time Complexity:** O(n) — but two passes
- **Space Complexity:** O(1)

```cpp
ListNode* middleNode(ListNode* head) {
    int n = 0;
    ListNode* temp = head;
    while (temp != NULL) {
        n++;
        temp = temp->next;
    }
    temp = head;
    for (int i = 0; i < n / 2; i++) {
        temp = temp->next;
    }
    return temp;
}
```

---

## Approach 2: Slow & Fast Pointers (Optimal — Tortoise and Hare)
**Idea:** Move `slow` one step and `fast` two steps at a time. When `fast` reaches the end (or one before it), `slow` is exactly at the middle — single pass, no need to know the length upfront.

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1)

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

**Walkthrough on `[1,2,3,4,5,6]`:**
```
start: slow=1, fast=1
step1: slow=2, fast=3
step2: slow=3, fast=5
step3: slow=4, fast=NULL (fast->next after 6 is NULL, loop condition fails)
return slow = 4 ✅ (correctly the second middle for even length)
```

---

## Key Takeaways / Patterns
- **Slow & Fast pointers (Tortoise and Hare)** is one of the most reusable linked-list patterns — also used for cycle detection, finding the start of a cycle, and finding the nth node from the end.
- The loop condition `fast != NULL && fast->next != NULL` is what automatically handles both odd and even length lists correctly — no separate case-handling needed.
- Trades the two-pass brute force for one pass by moving `fast` twice as fast, since the ratio directly gives the midpoint when `fast` finishes.

## Edge Cases Considered
- [ ] Single node list (returns that node immediately)
- [ ] Two node list (returns second node)
- [ ] Odd length list
- [ ] Even length list (must return second middle, not first)

## Related Problems
- Linked List Cycle (same slow/fast pattern)
- Linked List Cycle II (find start of cycle)
- Remove Nth Node From End of List
- Palindrome Linked List (uses middle-finding as a subroutine)

## Mistakes I Made (if any)
- None — clean solve using the standard slow/fast pattern on first try.
