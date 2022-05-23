---
title: "746_Min_Cost_Climbing_Stairs"
date: 2022-05-23T22:43:58+08:00
categories:
- LeetCode
- Dynamic Programming
- Recursion
tags:
- LeetCode
- Dynamic Programming
keywords:
- LeetCode
- Dynamic Programming
- C++
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
```
class Solution {
    /* solution 1: recursion + memorization
     * dp[i] = min cost to reach the i-th step(not including climbing i-th)
     *       = min(dp[i-1]+cost[i-1], dp[i-2]+cost[i-2]);
     *
     *  pos  i-2  i-1  i
     *        |--------^  If I pay the cost[i-2], I can climb 2 steps to reach i
     *             |---^  If I pay the cost[i-1], I can climb 1 steps to reach i
     */
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size() + 1, 0);
        return find_cost(cost, dp, dp.size() - 1);
    }
    int find_cost(vector<int>& cost, vector<int>& dp, int i){
        if(i <= 1) 
            return 0;
        if(dp[i] > 0)
            return dp[i];
        dp[i] = min(find_cost(cost, dp, i-1) + cost[i-1],
                   find_cost(cost, dp, i-2) + cost[i-2]);
        return dp[i];
    }
};
```
