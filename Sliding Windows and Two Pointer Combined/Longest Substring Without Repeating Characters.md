# Longest Substring Without Repeating Characters

**Topic:** Sliding Window / Hashing<br>
**Difficulty:** Medium<br>
**Link:** [LeetCode — Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/?utm_source=chatgpt.com)<br>
**Date solved:** 2026-09-04<br>
**Status:** ✅ Solved<br>

---

## Problem Statement

Given a string `s`, find the length of the **longest substring without repeating characters**.

A substring must contain characters that are **unique**.

### Example

```text
Input:  s = "abcabcbb"
Output: 3

Explanation:
The longest substring without repeating characters is "abc".
```

---

## Approach

Use the **Sliding Window** technique with an `unordered_map`.

We maintain:

* `left` → start of the current window
* `right` → end of the current window
* `mp` → stores the characters currently present in the window
* `max_cnt` → maximum length found so far

The window represents:

```text
s[left ... right]
```

---

## Algorithm

For every character at index `right`:

1. Check whether the character already exists in the current window.
2. If it exists, remove characters from the left until the duplicate character is removed.
3. Insert the current character into the map.
4. Calculate the current window length.
5. Update `max_cnt`.

---

## Code

```cpp
class Solution {

public:

    int lengthOfLongestSubstring(string s) {

        int n = s.length();
        int left = 0;
        int max_cnt = 0;

        unordered_map<int, int> mp;

        for(int right = 0; right < n; right++){

            if(mp.find(s[right]) != mp.end()){

                while(mp.count(s[right])){
                    mp.erase(s[left]);
                    left++;
                }
            }

            mp[s[right]]++;

            max_cnt = max(max_cnt, right - left + 1);
        }

        return max_cnt;
    }
};
```

---

## Dry Run

### Input

```text
s = "1R1T7"
```

String with indices:

```text
Index:   0   1   2   3   4
         1   R   1   T   7
```

### Initial State

```text
left = 0
max_cnt = 0
mp = {}
```

---

### `right = 0`

Character:

```text
s[0] = '1'
```

`'1'` is not present.

Insert:

```text
mp = {'1': 1}
```

Window:

```text
1
↑
L,R
```

Length:

```text
0 - 0 + 1 = 1
```

```text
max_cnt = 1
```

---

### `right = 1`

Character:

```text
s[1] = 'R'
```

`'R'` is not present.

Insert:

```text
mp = {'1': 1, 'R': 1}
```

Window:

```text
1 R
↑   ↑
L   R
```

Length:

```text
1 - 0 + 1 = 2
```

```text
max_cnt = 2
```

---

### `right = 2`

Character:

```text
s[2] = '1'
```

`'1'` already exists in the map.

Therefore:

```cpp
while(mp.count(s[right]))
```

runs.

Current state:

```text
left = 0
right = 2
mp = {'1': 1, 'R': 1}
```

#### First iteration

```text
s[left] = s[0] = '1'
```

Erase:

```text
mp.erase('1')
```

Move `left`:

```text
left = 1
```

Map:

```text
{'R': 1}
```

Now:

```text
mp.count(s[right])
```

means:

```text
mp.count('1')
```

which is `0`.

So the loop stops.

Then insert the current `'1'`:

```text
mp['1']++
```

Map:

```text
{'R': 1, '1': 1}
```

Current window:

```text
1 R 1
  ↑   ↑
  L   R
```

Actual valid window:

```text
R 1
↑ ↑
L R
```

Length:

```text
2 - 1 + 1 = 2
```

`max_cnt` remains:

```text
2
```

---

### `right = 3`

Character:

```text
s[3] = 'T'
```

`'T'` is not present.

Insert:

```text
mp = {'R': 1, '1': 1, 'T': 1}
```

Window:

```text
R 1 T
↑     ↑
L     R
```

Length:

```text
3 - 1 + 1 = 3
```

```text
max_cnt = 3
```

---

### `right = 4`

Character:

```text
s[4] = '7'
```

`'7'` is not present.

Insert:

```text
mp = {'R': 1, '1': 1, 'T': 1, '7': 1}
```

Window:

```text
R 1 T 7
↑       ↑
L       R
```

Length:

```text
4 - 1 + 1 = 4
```

Therefore:

```text
max_cnt = 4
```

---

## Final Answer

For:

```text
s = "1R1T7"
```

the longest substring without repeating characters is:

```text
"R1T7"
```

Therefore:

```text
Answer = 4
```

---

## Why Does `left` Become 1?

At `right = 2`, the character is another `'1'`.

Before processing it:

```text
1 R 1
↑   ↑
L   R
```

The duplicate is `'1'`.

Your loop is:

```cpp
while(mp.count(s[right])){
    mp.erase(s[left]);
    left++;
}
```

First:

```text
s[left] = '1'
```

So:

```cpp
mp.erase('1');
left++;
```

Now:

```text
left = 1
```

Since `'1'` is no longer in the map, the loop stops.

The new window is:

```text
R 1
↑ ↑
L R
```

This is why `left` becomes `1`.

---

## Important Observation About `unordered_map`

Although you declared:

```cpp
unordered_map<int, int> mp;
```

you are mainly using it to check whether a character exists:

```cpp
mp.find(s[right])
mp.count(s[right])
```

and storing:

```cpp
mp[s[right]]++;
```

The frequency value isn't actually being used to make decisions.

Therefore, an `unordered_set<char>` would be more natural for this exact approach.

However, your `unordered_map` solution works correctly.

---

## Time Complexity

Each character is:

* Added to the map once.
* Removed from the map at most once.

Therefore:

```text
Time Complexity: O(n)
```

The map stores at most the number of distinct characters in the current window:

```text
Space Complexity: O(min(n, character_set_size))
```

For a fixed character set, this is effectively:

```text
Space Complexity: O(1)
```

---

## Key Takeaways

* This is a classic **Sliding Window** problem.
* `right` expands the window.
* `left` shrinks the window when a duplicate is found.
* The current window is:

  ```cpp
  s[left ... right]
  ```
* Window length:

  ```cpp
  right - left + 1
  ```
* `unordered_map`/`unordered_set` gives average `O(1)` lookup.
* Each character is added and removed at most once, giving `O(n)` time.
* When a duplicate is encountered, remove elements from the left **until the duplicate is no longer present**.

---

## Edge Cases

### Empty String

```text
s = ""
```

Answer:

```text
0
```

### One Character

```text
s = "a"
```

Answer:

```text
1
```

### All Characters Same

```text
s = "aaaa"
```

Answer:

```text
1
```

### All Characters Unique

```text
s = "abcdef"
```

Answer:

```text
6
```

### Duplicate at the Beginning

```text
s = "abba"
```

The sliding window must correctly move `left` whenever a duplicate enters the window.

---

## Pattern

```text
Sliding Window + Hash Set/Map
```

General structure:

```cpp
for(right = 0; right < n; right++){

    // If window becomes invalid
    while(window is invalid){
        remove s[left];
        left++;
    }

    // Add current element

    // Update answer
}
```
