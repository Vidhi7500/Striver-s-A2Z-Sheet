# Reverse Linked List

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/reverse-linked-list/ (LeetCode 206)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given the `head` of a singly linked list, reverse the list and return the new head.

**Example:**
```
Input:  head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

---

## Approach 1: Iterative — Three Pointers (Optimal)
**Idea:** Walk through the list once, flipping each node's `next` pointer to point backward instead of forward. Three pointers are needed simultaneously: `prev` (the reversed portion built so far), `curr` (node being processed), and `Next` (a temporary save of `curr->next`, since it gets overwritten before we can move on).

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1) — in-place, only pointers used

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = NULL;
        ListNode* curr = head;
        ListNode* Next = NULL;
        while (curr != NULL) {
            Next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = Next;
        }
        return prev;
    }
};
```

**Walkthrough on `[1,2,3]`:**
```
start: prev=NULL, curr=1
step1: Next=2, 1->next=NULL, prev=1, curr=2   →  1->NULL
step2: Next=3, 2->next=1,    prev=2, curr=3   →  2->1->NULL
step3: Next=NULL, 3->next=2, prev=3, curr=NULL → 3->2->1->NULL
loop ends → return prev = node 3, i.e. list is 3->2->1 ✅
```

---

## Approach 2: Recursive (Alternative)
**Idea:** Recurse to the end of the list first, then as the call stack unwinds, make each node's next node point back to it.

- **Time Complexity:** O(n)
- **Space Complexity:** O(n) — recursion call stack

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == NULL || head->next == NULL) return head;
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return newHead;
    }
};
```

---

## Key Takeaways / Patterns
- **The "save next before overwriting" trick** (`Next = curr->next` before `curr->next = prev`) is the core of nearly every linked-list mutation problem — forgetting this order loses the rest of the list permanently, since `curr->next` gets overwritten before you've captured it.
- Iterative version is O(1) space and generally preferred; recursive version is more elegant to read but costs O(n) stack space — worth mentioning both in an interview to show awareness of the tradeoff.
- This reversal logic is a building block for many other problems (reverse in groups of k, reverse between positions, palindrome check).

## Edge Cases Considered
- [ ] Empty list (`head == NULL`)
- [ ] Single node list
- [ ] Two node list
- [ ] Already-checked example with odd and even lengths

## Related Problems
- Reverse Linked List II (reverse between positions m and n)
- Reverse Nodes in k-Group
- Palindrome Linked List (uses reversal as a subroutine)
- Middle of the Linked List (same "pointer manipulation" family)

## Mistakes I Made (if any)
- None — the three-pointer pattern (`prev`, `curr`, `Next`) was applied correctly in the right order on first try.
