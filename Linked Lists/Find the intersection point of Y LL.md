# Intersection of Two Linked Lists

**Topic:** Linked List
**Difficulty:** Easy
**Link:** https://leetcode.com/problems/intersection-of-two-linked-lists/
**Date solved:** 2026-09-04
**Status:** ✅ Solved

---

## Problem Statement

Given the heads of two singly linked lists, find the node at which the two lists intersect.

If the two linked lists do not intersect, return `NULL`.

**Important:** The intersection is based on **node identity**, not node value. Two nodes having the same value do not necessarily mean the lists intersect.

**Example:**

```text
List A: 1 → 9 → 1 → 2 → 4
                    ↘
                      2 → 4
                    ↗
List B:     3 ───────

Output: Node with value 2
```

Another example:

```text
List A: 2 → 6 → 4
List B: 1 → 5

Output: NULL
```

---

## Approach: Optimal (Length Difference)

**Idea:** The two linked lists may have different lengths. If we start comparing them from their heads, the pointers will not be at the same distance from the end.

To solve this:

1. Find the length of both linked lists.
2. Move the pointer of the longer list forward by the difference in lengths.
3. Now both pointers have the same number of nodes remaining.
4. Move both pointers one step at a time.
5. The first node where `headA == headB` is the intersection node.
6. If both become `NULL`, there is no intersection.

### Why Do We Align the Lists?

Consider:

```text
A: 1 → 9 → 1 → 2 → 4
B:     3 → 2 → 4
```

Lengths:

```text
A = 5
B = 3
```

Difference:

```text
5 - 3 = 2
```

So move `headA` forward by `2` nodes:

```text
A: 1 → 9 → 1 → 2 → 4
          ↓       ↓
After:    1 → 2 → 4

B:        3 → 2 → 4
```

Now both pointers have the same number of nodes remaining:

```text
A: 1 → 2 → 4
B: 3 → 2 → 4
```

Then move both together:

```text
A: 1 → 2
B: 3 → 2
       ↑
   Intersection
```

Both pointers eventually point to the **same node object**, so `headA == headB` becomes true.

---

## Code

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

class Solution {
public:
    int length(ListNode* head) {
        int cnt = 0;
        ListNode* curr = head;

        while (curr != NULL) {
            cnt++;
            curr = curr->next;
        }

        return cnt;
    }

    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        if (headA == NULL || headB == NULL)
            return NULL;

        int n = length(headA);
        int m = length(headB);

        // Align both lists
        while (n > m) {
            headA = headA->next;
            --n;
        }

        while (m > n) {
            headB = headB->next;
            --m;
        }

        // Find intersection
        while (headA != headB && headA != NULL && headB != NULL) {
            headA = headA->next;
            headB = headB->next;
        }

        return headA;
    }
};
```

---

## Dry Run

Consider:

```text
A: 1 → 9 → 1 → 2 → 4
B: 3 → 2 → 4
```

### Step 1: Calculate lengths

```text
n = 5
m = 3
```

### Step 2: Align the lists

Since `n > m`:

```cpp
while (n > m) {
    headA = headA->next;
    --n;
}
```

First iteration:

```text
n = 4
A: 9 → 1 → 2 → 4
```

Second iteration:

```text
n = 3
A: 1 → 2 → 4
```

Now:

```text
n = m = 3
```

Both lists are aligned.

### Step 3: Compare nodes

```text
A: 1 → 2 → 4
B: 3 → 2 → 4
```

First comparison:

```text
1 != 3
```

Move both:

```text
A: 2 → 4
B: 2 → 4
```

Now:

```text
headA == headB
```

So the intersection node is returned.

---

## Why `headA != headB` Instead of `headA->val != headB->val`?

This is very important.

The problem asks for the **intersection node**, not just a node with the same value.

For example:

```text
A: 1 → 2 → 3
B: 4 → 2 → 5
```

The two `2` nodes may be completely different nodes in memory.

Therefore:

```cpp
headA->val == headB->val
```

does **not** prove that the lists intersect.

We need:

```cpp
headA == headB
```

which checks whether both pointers refer to the **same node**.

---

## Time & Space Complexity

### Finding Lengths

Each list is traversed once:

```text
O(n) + O(m)
```

### Aligning Lists

At most `|n - m|` nodes are skipped:

```text
O(|n - m|)
```

### Finding Intersection

At most `min(n, m)` nodes are traversed:

```text
O(min(n, m))
```

Therefore:

* **Time Complexity:** `O(n + m)`
* **Space Complexity:** `O(1)`

No additional data structure is used.

---

## Key Takeaways / Patterns

* When comparing two linked lists, **align them based on their lengths** if using the length-difference approach.
* The longer list must be advanced by exactly:

```cpp
abs(n - m)
```

* `headA == headB` checks **node identity**, which is what intersection problems require.
* Equal values do not necessarily mean an intersection.
* Once both lists have the same number of nodes remaining, we can move both pointers together.
* If there is no intersection, both pointers eventually become `NULL`.
* This is an **O(n + m) time and O(1) space** solution.

---

## Edge Cases Considered

* [x] One or both lists are empty
* [x] Both lists are of the same length
* [x] One list is longer than the other
* [x] Intersection occurs at the head
* [x] Intersection occurs somewhere in the middle
* [x] Intersection occurs at the last node
* [x] Lists do not intersect
* [x] Lists contain duplicate values but different nodes

---

## Related Problems

* Linked List Cycle
* Linked List Cycle II
* Remove Nth Node From End of List
* Middle of the Linked List
* Merge Two Sorted Lists

---

## Mistakes I Made (if any)

* Initially, I moved the longer list forward by only **one node**:

```cpp
if (n > m) {
    headA = headA->next;
    --n;
}
```

This was incorrect because the longer list may be longer by more than one node.

For example:

```text
n = 5
m = 3
```

The difference is `2`, so `headA` needs to move forward **two nodes**, not one.

The corrected approach uses:

```cpp
while (n > m) {
    headA = headA->next;
    --n;
}
```

and similarly for `headB`.

* Another important lesson is to compare:

```cpp
headA == headB
```

instead of:

```cpp
headA->val == headB->val
```

because intersection means the **same node**, not merely the same value.
