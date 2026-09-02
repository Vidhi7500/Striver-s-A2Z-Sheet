# Best Time to Buy and Sell Stock

**Topic:** Arrays  
**Difficulty:** Easy  
**Link:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock/ (LeetCode 121)  
**Date solved:** 2026-09-02  
**Status:** ✅ Solved

---

## Problem Statement
Given an array `prices` where `prices[i]` is the stock price on day `i`, find the maximum profit achievable by buying on one day and selling on a later day. If no profit is possible, return `0`.

**Example:**
```
Input:  prices = [7,1,5,3,6,4]
Output: 5   (buy at 1, sell at 6)

Input:  prices = [7,6,4,3,1]
Output: 0   (prices only fall — no profitable transaction exists)
```

---

## Approach: Track Running Minimum (Optimal)
**Idea:** Walk through the prices once, keeping track of the lowest price seen so far (`price`, the best possible buy point up to now). At each day, compute the profit if sold today against that running minimum, and update the best profit seen. Update the running minimum after computing profit for the current day.

- **Time Complexity:** O(n) — single pass
- **Space Complexity:** O(1)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int max_profit = INT_MIN;
        int profit = 0, price = prices[0];
        for (int i = 0; i < n; i++) {
            profit = prices[i] - price;
            max_profit = max(max_profit, profit);
            price = min(price, prices[i]);
        }
        return max_profit;
    }
};
```

**Walkthrough on `[7,6,4,3,1]` (strictly decreasing — no profit possible):**
```
price=7
i=0: profit=7-7=0,  max_profit=0,  price=7
i=1: profit=6-7=-1, max_profit=0,  price=6
i=2: profit=4-6=-2, max_profit=0,  price=4
i=3: profit=3-4=-1, max_profit=0,  price=3
i=4: profit=1-3=-2, max_profit=0,  price=1
return 0 ✅
```

---

## Key Takeaways / Patterns
- **The "buy today, sell today" case at `i=0` is what implicitly guarantees the `0` floor.** Since `price` is initialized to `prices[0]`, the very first iteration always computes `profit = 0`, which sets `max_profit` to at least `0` before any negative profits can appear. This is a subtle but important reason the code correctly avoids returning a negative profit without needing an explicit `max(max_profit, 0)` at the end.
- Same "local vs global tracking" shape as Maximum Subarray (Kadane's) — here it's "best profit if sold today" (local) vs "best profit overall" (global), using a running **minimum** instead of a running sum.
- Order of operations matters: profit is computed using the minimum price **before** today, then the minimum is updated to include today — this correctly prevents "buying and selling on the same day using today's price as both" from being miscounted as a real transaction advantage (it still works out to profit 0, which is valid, not exploited).

## Edge Cases Considered
- [x] Strictly decreasing prices — correctly returns 0
- [ ] Strictly increasing prices (buy first day, sell last day)
- [ ] Single day (`prices.size() == 1`) — profit is 0, buy and sell same day
- [ ] Prices with a dip then a bigger rise later

## Related Problems
- Maximum Subarray (Kadane's — same local/global tracking pattern)
- Best Time to Buy and Sell Stock II (multiple transactions allowed)
- Best Time to Buy and Sell Stock III / IV (limited number of transactions — DP)

## Mistakes I Made (if any)
- None — the running-minimum approach correctly handled the "no profit possible" edge case without needing an extra explicit check.
