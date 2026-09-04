# Remove Nth Node From End of List

**Topic:** Linked List<br>
**Difficulty:** Medium<br>
**Link:** [LeetCode — Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/?utm_source=chatgpt.com)<br>
**Date solved:** 2026-09-04<br>
**Status:** ✅ Solved<br>

---

## Problem Statement

Given the head of a linked list, remove the `n`th node from the end of the list and return its head.

### Example

```text
Input:  1 -> 2 -> 3 -> 4 -> 5, n = 2

Output: 1 -> 2 -> 3 -> 5
```

The `2`nd node from the end is `4`, so we remove it.

---

## Approach

### Step 1: Find the length of the linked list

Traverse the entire linked list and calculate its length.

```cpp
int len = length(head);
```

For:

```text
1 -> 2 -> 3 -> 4 -> 5
```

we get:

```text
len = 5
```

---

### Step 2: Convert nth node from the end to position from the beginning

If the list has length `len` and we want the `n`th node from the end:

```cpp
n = len - n;
```

For:

```text
1 -> 2 -> 3 -> 4 -> 5
n = 2
```

we get:

```text
n = 5 - 2
  = 3
```

This means we need to reach the node **before the node that needs to be deleted**.

---

### Step 3: Handle deletion of the head

If:

```cpp
n == 0
```

the node to remove is the head.

Example:

```text
1 -> 2 -> 3 -> 4 -> 5
n = 5
```

Since:

```text
len - n = 5 - 5 = 0
```

we delete the head:

```cpp
ListNode* temp = head;
head = head->next;
delete temp;
return head;
```

---

### Step 4: Reach the node before the target

```cpp
ListNode* curr = head;

while(n > 1 && curr != NULL){
    curr = curr->next;
    n--;
}
```

We stop at the node **just before** the node that needs to be deleted.

For:

```text
1 -> 2 -> 3 -> 4 -> 5
```

and `n = 2`:

```text
len = 5
n = 5 - 2 = 3
```

We move:

```text
curr
 ↓
1 -> 2 -> 3 -> 4 -> 5

     curr
      ↓
1 -> 2 -> 3 -> 4 -> 5

          curr
           ↓
1 -> 2 -> 3 -> 4 -> 5
```

Now `curr` points to `3`, which is immediately before `4`.

---

### Step 5: Delete the target node

```cpp
ListNode* temp = curr->next;
curr->next = curr->next->next;
delete temp;
```

Before:

```text
1 -> 2 -> 3 -> 4 -> 5
          ↑    ↑
        curr  temp
```

After:

```text
1 -> 2 -> 3 ------> 5
          ↑
        curr
```

The node `4` is removed.

---

## Code

```cpp
class Solution {
public:
    int length(ListNode* head){
        int cnt = 0;
        ListNode* curr = head;

        while(curr != NULL){
            cnt++;
            curr = curr->next;
        }

        return cnt;
    }

    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head == NULL || head->next == NULL)
            return NULL;

        int len = length(head);

        n = len - n;

        // Delete head
        if(n == 0){
            ListNode* temp = head;
            head = head->next;
            delete temp;
            return head;
        }

        ListNode* curr = head;

        // Reach node before the target node
        while(n > 1 && curr != NULL){
            curr = curr->next;
            n--;
        }

        // Delete target node
        ListNode* temp = curr->next;
        curr->next = curr->next->next;
        delete temp;

        return head;
    }
};
```

---

## Dry Run

### Input

```text
head = [1,2,3,4,5]
n = 2
```

### Find length

```text
len = 5
```

### Convert position

```text
n = len - n
n = 5 - 2
n = 3
```

### Move `curr`

Initially:

```text
curr -> 1
```

Loop:

```cpp
while(n > 1)
```

First iteration:

```text
curr -> 2
n = 2
```

Second iteration:

```text
curr -> 3
n = 1
```

Loop stops.

So:

```text
1 -> 2 -> 3 -> 4 -> 5
          ↑
        curr
```

### Delete `curr->next`

```cpp
temp = curr->next;
```

So:

```text
temp -> 4
```

Then:

```cpp
curr->next = curr->next->next;
```

Now:

```text
1 -> 2 -> 3 -> 5
```

Finally:

```cpp
delete temp;
```

Node `4` is freed.

### Output

```text
1 -> 2 -> 3 -> 5
```

---

## Edge Cases

### 1. Empty list

```text
head = NULL
```

Return:

```cpp
NULL
```

---

### 2. Single-node list

```text
1
```

Removing the only node gives:

```text
NULL
```

Handled by:

```cpp
if(head == NULL || head->next == NULL)
    return NULL;
```

---

### 3. Remove the first node

```text
1 -> 2 -> 3 -> 4
n = 4
```

```text
len - n = 0
```

So the head is deleted.

Result:

```text
2 -> 3 -> 4
```

---

### 4. Remove the last node

```text
1 -> 2 -> 3 -> 4
n = 1
```

We reach node `3` and delete node `4`.

Result:

```text
1 -> 2 -> 3
```

---

## Why `while(n > 1)`?

We need `curr` to stop at the **previous node** of the node we want to delete.

For:

```text
1 -> 2 -> 3 -> 4 -> 5
```

if `4` needs to be deleted:

```text
1 -> 2 -> 3 -> 4 -> 5
          ↑
        curr
```

Therefore, `curr` must point to `3`.

That's why we stop when:

```cpp
n == 1
```

rather than moving until `n == 0`.

---

## Mistake I Made

Initially, the traversal condition was:

```cpp
while(n == 1 && curr != NULL)
```

This was incorrect because the loop would execute only when `n` was already `1`.

The correct condition is:

```cpp
while(n > 1 && curr != NULL)
```

This allows us to move `curr` until it reaches the node immediately before the target.

---

## Important Pointer Safety

Before doing:

```cpp
curr->next
```

we must make sure:

```cpp
curr != NULL
```

Otherwise, we can get:

```text
runtime error:
member access within null pointer of type 'ListNode'
```

Similarly, after deleting a node:

```cpp
delete temp;
```

we must not access:

```cpp
temp->next
temp->val
```

because `temp` now points to freed memory.

---

## Time Complexity

### `length()`

```text
O(n)
```

### Finding the target

```text
O(n)
```

### Deleting the node

```text
O(1)
```

Overall:

```text
Time: O(n)
Space: O(1)
```

---

## Key Takeaways

* First calculate the linked-list length.
* Convert `n`th node from the end into a position from the beginning using:

  ```text
  len - n
  ```
* Handle the head separately when:

  ```text
  len - n == 0
  ```
* Stop at the node **before** the node to delete.
* Reconnect the previous node to the target's next node:

  ```cpp
  curr->next = curr->next->next;
  ```
* Always `delete` the removed node to avoid a memory leak.
* Never access a node after `delete`.

---

## Related Problems

* Remove Linked List Elements
* Delete the Middle Node of a Linked List
* Middle of the Linked List
* Linked List Cycle
* Intersection of Two Linked Lists
* Reverse Linked List

---

## Pattern

**Linked List → Find Previous Node → Reconnect Pointers → Delete Node**

This is a useful pattern for many linked-list deletion problems.
