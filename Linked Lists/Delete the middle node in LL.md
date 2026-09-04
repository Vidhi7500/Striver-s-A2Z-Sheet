# Delete Middle Node of a Linked List

**Topic:** Linked List
**Difficulty:** Easy
**Link:** https://www.geeksforgeeks.org/problems/delete-middle-of-linked-list/1
**Date solved:** 2026-09-04
**Status:** ✅ Solved

---

## Problem Statement

Given the head of a singly linked list, delete the **middle node** of the linked list and return the head of the modified list.

If the linked list contains an **even number of nodes**, there are two middle nodes. In that case, delete the **second middle node**.

If the linked list contains only one node, return `NULL`.

**Example:**

```text
Input:  1 → 2 → 3 → 4 → 5
Output: 1 → 2 → 4 → 5
```

Here, `3` is the middle node and is deleted.

For an even-sized list:

```text
Input:  1 → 2 → 3 → 4 → 5 → 6
Output: 1 → 2 → 3 → 5 → 6
```

The two middle nodes are `3` and `4`. The **second middle node `4`** is deleted.

---

## Approach: Optimal (Slow and Fast Pointers)

**Idea:** Use two pointers:

* `slow` → moves **one node at a time**
* `fast` → moves **two nodes at a time**

When `fast` reaches the end, `slow` will point to the middle node.

However, we also need the node **before** the middle node so that we can remove the middle node from the linked list.

After finding the middle:

1. Start another pointer `curr` from `head`.
2. Find the node whose `next` is `slow`.
3. Change its `next` pointer to skip `slow`.
4. Delete `slow`.

---

## Code

```cpp
class Solution {
public:
    Node* deleteMid(Node* head) {
        // Empty list or single node
        if (head == NULL || head->next == NULL)
            return NULL;

        Node* slow = head;
        Node* fast = head;

        // Find the middle node
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Find the node before the middle
        Node* curr = head;

        while (curr->next != slow) {
            curr = curr->next;
        }

        // Remove the middle node
        curr->next = curr->next->next;

        // Free memory
        delete slow;

        return head;
    }
};
```

---

## How Slow and Fast Pointers Find the Middle

Consider:

```text
1 → 2 → 3 → 4 → 5
```

Initially:

```text
slow = 1
fast = 1
```

### Iteration 1

```text
slow → 2
fast → 3
```

### Iteration 2

```text
slow → 3
fast → 5
```

Now:

```text
fast->next == NULL
```

so the loop stops.

Therefore:

```text
slow = 3
```

`3` is the middle node.

---

## Even Number of Nodes

Consider:

```text
1 → 2 → 3 → 4 → 5 → 6
```

Initially:

```text
slow = 1
fast = 1
```

After the first iteration:

```text
slow = 2
fast = 3
```

After the second iteration:

```text
slow = 3
fast = 5
```

After the third iteration:

```text
slow = 4
fast = NULL
```

Therefore:

```text
slow = 4
```

The second middle node is selected.

This is exactly what the problem requires for an even-sized linked list.

---

## Deleting the Middle Node

Finding the middle node is not enough.

Suppose:

```text
1 → 2 → 3 → 4 → 5
        ↑
       slow
```

We need the node **before** `slow`.

So we start:

```cpp
Node* curr = head;
```

and move until:

```cpp
curr->next == slow
```

Therefore:

```text
curr = 2
slow = 3
```

Then:

```cpp
curr->next = curr->next->next;
```

changes:

```text
2 → 3 → 4
```

into:

```text
2 ─────→ 4
```

So the final list becomes:

```text
1 → 2 → 4 → 5
```

Finally:

```cpp
delete slow;
```

frees the memory occupied by the deleted node.

---

## Why Do We Need `curr`?

You could find the middle using `slow`, but to delete a node from a singly linked list, you normally need access to the node **before it**.

For example:

```text
1 → 2 → 3 → 4 → 5
        ↑
       slow
```

To remove `3`, we need:

```text
2 → 4
```

Therefore, we need the pointer to `2`.

That's why we use:

```cpp
while (curr->next != slow) {
    curr = curr->next;
}
```

After this loop:

```text
curr → 2
slow → 3
```

and we can bypass `slow`.

---

## Time & Space Complexity

The list is traversed to find the middle and then traversed again to find its predecessor.

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)`

Even though there are two traversals, the total work is still linear:

```text
O(n) + O(n) = O(n)
```

---

## Key Takeaways / Patterns

* **Slow and fast pointers** are a very common linked-list pattern.
* `slow` moves one step while `fast` moves two steps.
* When `fast` reaches the end, `slow` is at the middle.
* For an even-sized list, this implementation selects the **second middle node**.
* To delete a node from a singly linked list, we generally need the node **before it**.
* `curr->next = curr->next->next` is the standard pattern for skipping/removing a node.
* `delete slow` releases the memory occupied by the removed node.
* Always handle the **empty list and single-node list** separately.

---

## Edge Cases Considered

* [x] Empty linked list → `NULL`
* [x] Single node → return `NULL`
* [x] Two nodes → delete the second node
* [x] Odd number of nodes → delete the exact middle
* [x] Even number of nodes → delete the second middle
* [x] Middle node is connected to the remaining list correctly

**Examples:**

```text
[1] → []

[1,2] → [1]

[1,2,3] → [1,2]

[1,2,3,4] → [1,2,3]

[1,2,3,4,5] → [1,2,4,5]

[1,2,3,4,5,6] → [1,2,3,5,6]
```

---

## Related Problems

* Middle of the Linked List
* Delete Node in a Linked List
* Remove Nth Node From End of List
* Linked List Cycle
* Intersection of Two Linked Lists

---

## Mistakes I Made (if any)

* No major logical mistake in the final solution.
* An important thing to remember is that finding the middle node and **deleting** it are two separate tasks.
* The `slow` pointer gives us the middle node, but because this is a singly linked list, we also need `curr` to find the node immediately before `slow`.
* The condition:

```cpp
while (curr->next != slow)
```

is safe because the problem guarantees that the list has at least two nodes after the initial check:

```cpp
if (head == NULL || head->next == NULL)
    return NULL;
```

* The `delete slow` statement is important in C++ to release the memory of the removed node.
