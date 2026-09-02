# Linked List Cycle II

**Topic:** Linked List  
**Difficulty:** Medium  
**Link:** https://leetcode.com/problems/linked-list-cycle-ii/ (LeetCode 142)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `NULL`.

**Example:**
```
Input:  head = [3,2,0,-4], tail connects to node at index 1
Output: node at index 1 (the node where the cycle starts)

Input:  head = [1,2], no cycle
Output: NULL
```

---

## Approach: Floyd's Cycle Detection, Two Phases (Optimal)
**Idea:** Same slow/fast setup as detecting *whether* a cycle exists (LeetCode 141), but extended with a second phase to find *where* it starts.

**Phase 1 — Detect a meeting point:** Move `slow` by 1 and `fast` by 2 until they meet inside the cycle (or `fast` hits `NULL`, meaning no cycle).

**Phase 2 — Find the start:** Reset `slow` back to `head`, keep `fast` at the meeting point. Move both **one step at a time** — they are mathematically guaranteed to meet again exactly at the node where the cycle begins.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        if (head == NULL || head->next == NULL) return NULL;

        // Phase 1: find meeting point inside the cycle (if any)
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) break;
        }

        // No cycle — fast ran off the end instead of meeting slow
        if (fast == NULL || fast->next == NULL) return NULL;

        // Phase 2: find the start of the cycle
        slow = head;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }
        return slow;
    }
};
```

---

## Why Phase 2 Works (the math)
Let:
- `L` = distance from `head` to the start of the cycle
- `C` = length of the cycle
- `x` = distance from the cycle's start to the meeting point

By the time `slow` and `fast` meet, `slow` has traveled `L + x`, and `fast` has traveled exactly twice that: `2(L + x)`. Since `fast` also covers some whole number of extra full loops around the cycle compared to `slow`, the extra distance `fast` covered is a multiple of `C`. Working through the algebra gives:

```
L = (a multiple of C) - x
```

In other words, walking `L` steps from `head` lands on the cycle start — and so does walking `L` steps from the meeting point (since `L ≡ (some loops) - x (mod C)`, moving `L` steps from the meeting point wraps around to the same spot). That's exactly why resetting one pointer to `head` and moving both one step at a time makes them meet at the cycle's start.

---

## Key Takeaways / Patterns
- **Distinguish "loop broke because they met" vs "loop broke because fast hit the end"** — checking `fast == NULL || fast->next == NULL` right after phase 1 is what correctly tells "no cycle" apart from "found a cycle." Easy to forget this check and misdiagnose a cycle-free list.
- The two-phase Floyd's algorithm is a direct extension of cycle *detection* (LeetCode 141) — detection is really just "phase 1 only, return true/false instead of continuing."
- Good to be able to sketch the math proof out loud in an interview — it shows understanding beyond memorized code.

## Edge Cases Considered
- [ ] Empty list / single node with no cycle
- [ ] No cycle at all (returns NULL correctly via the fast==NULL/fast->next==NULL check)
- [ ] Cycle starting exactly at `head`
- [ ] Cycle length of 1 (self-loop on the last node)

## Related Problems
- Linked List Cycle (detect only — LeetCode 141)
- Middle of the Linked List (same pointer skeleton)
- Find the Duplicate Number (array version of the same cycle-detection idea)

## Mistakes I Made (if any)
- None — correctly handled both the "no cycle" early exit and the two-phase meeting logic on first try.
