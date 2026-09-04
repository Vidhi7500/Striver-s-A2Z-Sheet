# Fruit Into Baskets

**Topic:** Sliding Window, Hash Map<br>
**Difficulty:** Medium<br>
**Link:** https://leetcode.com/problems/fruit-into-baskets/<br>
**Date solved:** 2026-09-04<br>
**Status:** ✅ Solved<br>

---

## Problem Statement

You are given an integer array `fruits` where `fruits[i]` represents the type of fruit in the `i-th` tree.

You have **two baskets**, and each basket can hold only one type of fruit.

Starting from any tree, you must collect fruit from every consecutive tree moving to the right.

You can stop when you encounter a third type of fruit.

Return the **maximum number of fruits** you can collect.

### In simple words

Find the **longest contiguous subarray containing at most 2 distinct values**.

---

## Examples

### Example 1

```text
Input:
fruits = [1,2,1]

Output:
3
```

Explanation:

```text
Window = [1,2,1]

Distinct fruit types = {1,2}
Number of fruits = 3
```

So we can collect all 3 fruits.

---

### Example 2

```text
Input:
fruits = [0,1,2,2]

Output:
3
```

Possible maximum window:

```text
[1,2,2]
```

It contains only two fruit types:

```text
1 → 1 fruit
2 → 2 fruits
```

Total:

```text
3
```

---

## Approach: Sliding Window + Hash Map

We maintain a sliding window:

```text
[left ........ right]
```

The window must contain **at most 2 distinct fruit types**.

We use:

```cpp
unordered_map<int, int> mp;
```

where:

```text
key   = fruit type
value = frequency of that fruit inside the current window
```

For example:

```text
fruits = [1,1,2,2]

mp:

1 → 2
2 → 2
```

There are only 2 distinct fruit types, so the window is valid.

---

## Algorithm

### Step 1: Expand the window

Move `right` from left to right.

For every fruit:

```cpp
mp[fruits[right]]++;
```

This adds the current fruit to the window.

---

### Step 2: Check if we have more than 2 fruit types

If:

```cpp
mp.size() > 2
```

then the current window contains more than two distinct fruit types.

So we move `left` forward until the window becomes valid again.

---

### Step 3: Remove fruits from the left

We decrease the frequency:

```cpp
mp[fruits[left]]--;
```

If its frequency becomes zero:

```cpp
if(mp[fruits[left]] == 0) {
    mp.erase(fruits[left]);
}
```

Then move `left`:

```cpp
left++;
```

---

### Step 4: Calculate the current window size

Once the window contains at most two fruit types:

```cpp
int cnt = 0;

for(auto it : mp) {
    cnt += it.second;
}
```

`cnt` represents the number of fruits in the current window.

Then:

```cpp
max_cnt = max(max_cnt, cnt);
```

updates the maximum answer.

---

## Code

```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        unordered_map<int, int> mp;
        
        int n = fruits.size();
        int left = 0;
        int max_cnt = 0;

        for(int right = 0; right < n; right++) {
            
            mp[fruits[right]]++;

            while(mp.size() > 2) {
                mp[fruits[left]]--;

                if(mp[fruits[left]] == 0) {
                    mp.erase(fruits[left]);
                }

                left++;
            }

            int cnt = 0;
            for(auto it : mp) {
                cnt += it.second;
            }

            max_cnt = max(max_cnt, cnt);
        }

        return max_cnt;
    }
};
```

---

# Dry Run

Consider:

```text
fruits = [1,2,3,2,2]
```

### Initial

```text
left = 0
max_cnt = 0
mp = {}
```

---

### right = 0

Fruit:

```text
1
```

Add it:

```text
mp = {
    1 → 1
}
```

Distinct types:

```text
1
```

Valid window:

```text
[1]
```

Count:

```text
1
```

```text
max_cnt = 1
```

---

### right = 1

Fruit:

```text
2
```

Add it:

```text
mp = {
    1 → 1,
    2 → 1
}
```

Distinct types:

```text
2
```

Window:

```text
[1,2]
```

Count:

```text
2
```

```text
max_cnt = 2
```

---

### right = 2

Fruit:

```text
3
```

Add it:

```text
mp = {
    1 → 1,
    2 → 1,
    3 → 1
}
```

Now:

```text
mp.size() = 3
```

We have 3 fruit types, so shrink from the left.

Remove `1`:

```text
mp[1]--
```

Frequency becomes zero:

```text
1 → 0
```

So erase it:

```text
mp.erase(1)
```

Move `left`:

```text
left = 1
```

Now:

```text
mp = {
    2 → 1,
    3 → 1
}
```

Window:

```text
[2,3]
```

Count:

```text
2
```

`max_cnt` remains:

```text
2
```

---

### right = 3

Fruit:

```text
2
```

Add it:

```text
mp = {
    2 → 2,
    3 → 1
}
```

Window:

```text
[2,3,2]
```

Count:

```text
2 + 1 = 3
```

Therefore:

```text
max_cnt = 3
```

---

### right = 4

Fruit:

```text
2
```

Add it:

```text
mp = {
    2 → 3,
    3 → 1
}
```

Window:

```text
[2,3,2,2]
```

Count:

```text
3 + 1 = 4
```

Therefore:

```text
max_cnt = 4
```

Final answer:

```text
4
```

---

# Why Does `mp.erase()` Happen Only When Frequency Becomes 0?

This is very important.

Suppose:

```text
fruits = [1,1,2]
```

The map is:

```text
1 → 2
2 → 1
```

If we remove one `1`:

```cpp
mp[1]--;
```

we get:

```text
1 → 1
2 → 1
```

We **cannot erase `1` yet** because there is still one `1` inside the window.

Only when:

```text
1 → 0
```

do we remove the key:

```cpp
mp.erase(1);
```

Therefore:

```cpp
mp[fruits[left]]--;

if(mp[fruits[left]] == 0) {
    mp.erase(fruits[left]);
}
```

is the correct way to maintain the frequency map.

---

# Important Observation

The problem can be reduced to:

> Find the longest contiguous subarray containing at most 2 distinct elements.

For example:

```text
[1,2,1,2,3]
```

The longest valid window is:

```text
[1,2,1,2]
```

because it contains:

```text
1
2
```

Only two distinct values.

When `3` appears, we need to shrink the window.

---

# Complexity

### Time Complexity

Your current implementation has:

```cpp
for(auto it : mp)
```

inside the main loop.

Since the map contains at most 2 keys after shrinking, this loop takes constant time.

Therefore:

```text
Time = O(n)
```

The `left` pointer also moves at most `n` times, and `right` moves `n` times.

So overall:

```text
O(n)
```

### Space Complexity

The map contains at most 3 entries temporarily before shrinking.

Therefore:

```text
Space = O(1)
```

More generally, using a hash map gives `O(k)` space where `k` is the number of distinct fruit types allowed in the window. Here `k = 2`.

---

# Key Takeaways

1. **Sliding Window** is useful for finding the longest/shortest valid contiguous subarray.

2. Use:

```cpp
unordered_map<int, int>
```

when you need both:

* distinct elements
* their frequencies

3. `mp.size()` gives the number of **distinct fruit types**, not the total number of fruits.

4. To remove an element from the sliding window:

```cpp
mp[fruits[left]]--;
```

5. Remove the key only when its frequency becomes zero:

```cpp
if(mp[fruits[left]] == 0)
    mp.erase(fruits[left]);
```

6. The window is valid when:

```cpp
mp.size() <= 2
```

7. The general sliding-window pattern is:

```text
Expand → Check condition → Shrink → Update answer
```

---

# Edge Cases Considered

### Empty array

```text
fruits = []
```

Answer:

```text
0
```

### One fruit

```text
fruits = [5]
```

Answer:

```text
1
```

### Only two types

```text
fruits = [1,2,1,2,2]
```

Answer:

```text
5
```

### More than two types

```text
fruits = [1,2,3,4]
```

The window must continuously shrink whenever it contains more than two distinct types.

---

# Common Mistakes

### Mistake 1: Using `multimap`

```cpp
multimap<int, int> mp;
```

A `multimap` can contain duplicate keys, so `size()` represents the total number of key-value entries rather than the number of distinct fruit types.

For this problem:

```cpp
unordered_map<int, int> mp;
```

is more appropriate.

---

### Mistake 2: Erasing the key immediately

Incorrect:

```cpp
mp.erase(fruits[left]);
```

This removes the entire fruit type even if multiple copies still exist in the window.

Correct:

```cpp
mp[fruits[left]]--;

if(mp[fruits[left]] == 0) {
    mp.erase(fruits[left]);
}
```

---

### Mistake 3: Counting map entries instead of frequencies

This:

```cpp
mp.size()
```

does **not** give the total number of fruits.

Example:

```text
mp:

1 → 4
2 → 3
```

Then:

```text
mp.size() = 2
```

but the total number of fruits is:

```text
4 + 3 = 7
```

---

# Related Pattern

This problem follows the general pattern:

```text
Longest Subarray with At Most K Distinct Elements
```

Here:

```text
K = 2
```

The general structure is:

```cpp
unordered_map<int, int> mp;

int left = 0;

for(int right = 0; right < n; right++) {

    mp[arr[right]]++;

    while(mp.size() > K) {

        mp[arr[left]]--;

        if(mp[arr[left]] == 0)
            mp.erase(arr[left]);

        left++;
    }

    // Current valid window
}
```

This pattern is worth remembering for many sliding-window problems.
