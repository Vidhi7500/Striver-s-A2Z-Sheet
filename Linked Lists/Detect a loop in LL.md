# Linked List Cycle

**Topic:** Linked List  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/linked-list-cycle/ (LeetCode 141)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved 

---

## Problem Statement
Given the `head` of a linked list, determine if it has a cycle in it (some node's `next` pointer loops back to a previous node).

**Example:**
```
Input:  head = [3,2,0,-4], with tail connecting back to node at index 1
Output: true

Input:  head = [1,2], no cycle
Output: false
```

---

## Approach 1: Hash Set (Brute Force)
**Idea:** Traverse the list, storing every visited node pointer in a set. If a node is encountered that's already in the set, a cycle exists.

- **Time Complexity:** O(n)
- **Space Complexity:** O(n) — set stores up to n node pointers

```cpp
bool hasCycle(ListNode *head) {
    unordered_set<ListNode*> visited;
    ListNode* curr = head;
    while (curr != NULL) {
        if (visited.count(curr)) return true;
        visited.insert(curr);
        curr = curr->next;
    }
    return false;
}
```

---

## Approach 2: Floyd's Cycle Detection — Slow & Fast Pointers (Optimal)
**Idea:** Same tortoise-and-hare setup as finding the middle node. If there's no cycle, `fast` reaches `NULL` naturally. If there **is** a cycle, `fast` (moving 2x speed) is guaranteed to eventually lap `slow` and land on the exact same node — because the gap between them shrinks by 1 every step once both are inside the loop.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1) — no extra data structure needed

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) return true;
        }
        return false;
    }
};
```

---

## Key Takeaways / Patterns
- Same **slow/fast pointer skeleton** as "Middle of the Linked List" — the only difference is the check inside the loop (`slow == fast` for cycle detection vs. just letting the loop end for the midpoint). Worth noticing how one pattern serves two different problems.
- The set approach is easier to reason about first, but the pointer trick is the expected O(1)-space answer in interviews — always mention both, lead with the tradeoff.
- This is "Floyd's Tortoise and Hare" algorithm by name — good to know the term, since follow-up problems (like finding *where* the cycle starts) build directly on it.

## Edge Cases Considered
- [ ] Empty list (`head == NULL`)
- [ ] Single node with no self-cycle
- [ ] Single node pointing to itself (self-loop)
- [ ] No cycle, list runs to `NULL` normally
- [ ] Cycle starting at the head itself

## Related Problems
- Linked List Cycle II (find the node where the cycle begins)
- Middle of the Linked List (same pointer pattern)
- Happy Number (cycle detection applied outside of linked lists)

## Mistakes I Made (if any)
- None — correct on first try, recognized the reused slow/fast skeleton from the middle-node problem.
