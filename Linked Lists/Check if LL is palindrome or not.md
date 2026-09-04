# Palindrome Linked List

**Topic:** Linked List
**Difficulty:** Easy
**Link:** https://leetcode.com/problems/palindrome-linked-list/
**Date solved:** 2026-09-04
**Status:** ✅ Solved

---

## Problem Statement

Given the head of a singly linked list, determine whether the linked list is a palindrome.

A linked list is a palindrome if it reads the same forward and backward.

**Example:**

```text
Input:  1 → 2 → 2 → 1
Output: true

Input:  1 → 2 → 3
Output: false

Input:  1 → 1 → 2 → 1
Output: false
```

---

## Approach: Copy the List + Reverse

**Idea:** Create a separate copy of the original linked list, reverse the copied list, and then compare it with the original list.

The important reason for creating a copy is that reversing a linked list modifies its `next` pointers.

If we directly do:

```cpp
reverse(head);
```

the original linked list gets modified, which means we can no longer properly compare the original list with its reversed version.

Instead:

```text
Original:
1 → 2 → 2 → 1

Copy:
1 → 2 → 2 → 1

Reverse Copy:
1 → 2 → 2 → 1
```

Then compare the original and reversed copy node by node.

---

## Steps

### Step 1: Create a Copy

```cpp
ListNode* curr = copyList(head);
```

The copied list contains completely new nodes.

For example:

```text
Original:
1 → 2 → 3 → NULL

Copy:
1 → 2 → 3 → NULL
```

The nodes are different objects in memory.

---

### Step 2: Reverse the Copy

```cpp
curr = reverse(curr);
```

Now:

```text
Original:
1 → 2 → 3 → NULL

Reversed Copy:
3 → 2 → 1 → NULL
```

The original list remains unchanged.

---

### Step 3: Compare Both Lists

```cpp
while(head != NULL && curr != NULL) {
    if(curr->val != head->val)
        return false;

    head = head->next;
    curr = curr->next;
}
```

If every corresponding value is equal, the linked list is a palindrome.

---

## Code

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* reverse(ListNode* head) {
        ListNode* curr = head;
        ListNode* prev = NULL;
        ListNode* Next = NULL;

        while(curr != NULL) {
            Next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = Next;
        }

        return prev;
    }

    ListNode* copyList(ListNode* head) {
        if(head == NULL)
            return NULL;

        ListNode* newHead = new ListNode(head->val);
        ListNode* curr = head->next;
        ListNode* copyCurr = newHead;

        while(curr != NULL) {
            copyCurr->next = new ListNode(curr->val);

            copyCurr = copyCurr->next;
            curr = curr->next;
        }

        return newHead;
    }

    bool isPalindrome(ListNode* head) {
        ListNode* curr = copyList(head);

        curr = reverse(curr);

        while(head != NULL && curr != NULL) {
            if(curr->val != head->val)
                return false;

            head = head->next;
            curr = curr->next;
        }

        return true;
    }
};
```

---

## Dry Run

Consider:

```text
Input:
1 → 1 → 2 → 1
```

### Step 1: Original List

```text
head
 ↓
1 → 1 → 2 → 1 → NULL
```

### Step 2: Create Copy

```text
Original:
1 → 1 → 2 → 1 → NULL

Copy:
1 → 1 → 2 → 1 → NULL
```

These are separate nodes.

### Step 3: Reverse the Copy

```text
Original:
1 → 1 → 2 → 1 → NULL

Reversed Copy:
1 → 2 → 1 → 1 → NULL
```

### Step 4: Compare

First:

```text
Original:       1
Reversed Copy:  1

1 == 1 ✅
```

Second:

```text
Original:       1
Reversed Copy:  2

1 != 2 ❌
```

Therefore:

```text
Output: false
```

---

## Why Do We Need `copyList()`?

Initially, you tried:

```cpp
ListNode* curr = reverse(head);
```

The problem is that `reverse()` changes the original list.

For example:

```text
Before reverse:

head
 ↓
1 → 1 → 2 → 1 → NULL
```

After reversing:

```text
curr
 ↓
1 → 2 → 1 → 1 → NULL
```

But `head` still points to the original first node, whose `next` pointer has now been changed:

```text
head
 ↓
1 → NULL
```

So comparing `head` and `curr` no longer compares the complete original list.

By creating a copy first:

```cpp
ListNode* curr = copyList(head);
curr = reverse(curr);
```

we reverse the **copy**, leaving the original list untouched.

---

## Why Does Comparing With the Reversed List Work?

A palindrome reads the same from both directions.

For example:

```text
Original:
1 → 2 → 3 → 2 → 1

Reversed:
1 → 2 → 3 → 2 → 1
```

They are identical.

But:

```text
Original:
1 → 2 → 3 → 4

Reversed:
4 → 3 → 2 → 1
```

They differ, so the list is not a palindrome.

Therefore, the problem can be reduced to:

> **Is the original list equal to its reversed version?**

---

## Time & Space Complexity

### `copyList()`

We visit every node once:

```text
O(n)
```

### `reverse()`

We visit every node once:

```text
O(n)
```

### Comparison

We visit every node once:

```text
O(n)
```

Therefore:

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)`

The extra space is `O(n)` because we create a complete copy of the linked list.

---

## Key Takeaways / Patterns

* Reversing a linked list **modifies its links**.
* If you need to preserve the original list, create a **deep copy** before reversing.
* A linked list is a palindrome if it is equal to its reversed version.
* `copyList()` creates completely new nodes rather than simply copying pointers.
* `reverse()` changes the `next` pointers of the copied nodes, leaving the original list untouched.
* This is a simple and intuitive approach, but it uses `O(n)` extra space.
* A more space-efficient solution uses the **slow/fast pointer + reverse second half** technique with `O(1)` extra space.

---

## Edge Cases Considered

* [x] Empty list → `true`
* [x] Single node → `true`
* [x] Two equal nodes → `true`
* [x] Two different nodes → `false`
* [x] Odd number of nodes
* [x] Even number of nodes
* [x] All nodes contain the same value
* [x] Palindrome with repeated values
* [x] Non-palindrome list

**Examples:**

```text
[] → true

[1] → true

[1,1] → true

[1,2] → false

[1,2,1] → true

[1,2,2,1] → true

[1,1,2,1] → false

[1,2,3,4] → false
```

---

## Related Problems

* Reverse Linked List
* Middle of the Linked List
* Intersection of Two Linked Lists
* Linked List Cycle
* Reorder List
* Reverse Linked List II

---

## Mistakes I Made (if any)

* Initially, I reversed the original list directly:

```cpp
ListNode* curr = reverse(head);
```

This caused a problem because `reverse()` modifies the `next` pointers of the original nodes.

* For example, after reversing:

```text
head → 1 → NULL
```

instead of still having access to the complete original list.

* The fix was to first create a separate copy:

```cpp
ListNode* curr = copyList(head);
```

and then reverse the copy:

```cpp
curr = reverse(curr);
```

* This allows the original list and reversed copy to be compared safely.
