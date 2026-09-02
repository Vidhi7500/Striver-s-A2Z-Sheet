# Find Length of Loop

**Topic:** Linked List  
**Difficulty:** Medium  
**Link:** https://www.geeksforgeeks.org/problems/find-length-of-loop/1  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given the `head` of a linked list that may contain a cycle, return the length of the cycle (number of nodes in it). Return `0` if there is no cycle.

**Example:**
```
Input:  head = [25, 14, 19, 33, 10, 21, 39, 90, 58, 45], tail connects to node at index 4
Output: 6   (the cycle contains 6 nodes)

Input:  head = [1, 2], no cycle
Output: 0
```

---

## Approach: Floyd's Cycle Detection + Count Around the Loop (Optimal)
**Idea:** Same slow/fast meeting-point setup as detecting a cycle (LeetCode 141). The moment `slow` and `fast` meet, both pointers are guaranteed to be **inside** the cycle. From there, keep one pointer fixed as a marker and walk another pointer forward, counting steps, until it comes back around to the marker — that count is exactly the cycle's length.

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```cpp
class Solution {
  public:
    int lengthOfLoop(Node *head) {
        Node* slow = head;
        Node* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                int cnt = 1;
                slow = slow->next;
                while (slow != fast) {
                    cnt++;
                    slow = slow->next;
                }
                return cnt;
            }
        }
        return 0;
    }
};
```

**Walkthrough (conceptual):**
```
Phase 1: slow/fast move at 1x/2x speed until they meet inside the cycle
Phase 2: fast stays put as the "anchor", slow walks forward counting steps
         until it laps back around to fast → that count = cycle length
```

---

## Key Takeaways / Patterns
- **Third variant of the same slow/fast skeleton** seen across this topic: detect a cycle (return bool) → find where it starts (two-phase, reset to head) → find its length (count steps back to the meeting point). Same foundation, three different follow-up questions — a good pattern to recognize as one family rather than three separate things to memorize.
- The counting trick works because once `slow == fast`, both are on the cycle; walking one of them forward until it returns to that exact node necessarily takes exactly `cycle length` steps, since it's now just walking a circular structure.
- No need to reset either pointer to `head` here (unlike Cycle II) — the length only depends on the loop's own size, not on the distance from `head`.

## Edge Cases Considered
- [x] No cycle — loop exits naturally via `fast == NULL`, returns 0
- [ ] Cycle length of 1 (single node looping to itself)
- [ ] Entire list is one big cycle (no tail before the loop)
- [ ] Very short list with a small loop

## Related Problems
- Linked List Cycle (detect only — LeetCode 141)
- Linked List Cycle II (find the start of the cycle — LeetCode 142)
- Middle of the Linked List (same pointer skeleton, different goal)

## Mistakes I Made (if any)
- None — correctly reused the meeting-point logic and added the counting loop cleanly.
