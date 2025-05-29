## Fibonacci like DP: dp[i] = f(dp[i-1], dp[i-2], ... , dp[i-m])
Leetcode 70. Climbing Stairs

> You are climbing a staircase. It takes n steps to reach the top.
> Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```cpp
int climbStairs(int n) {
    if (n <= 1) return n; 
    vector<int> dp(n + 1);
    dp[1] = 1;
    dp[2] = 2;
    for (int i = 3; i <= n; i++) { 
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

> You are climbing a staircase. It takes n steps to reach the top.
> Each time you can either climb 1, 2, ..., or m steps. In how many distinct ways can you climb to the top?

```cpp
int climbStairs(int n) {
    vector<int> dp(n + 1, 0);
    dp[0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) { 
            if (i - j >= 0) dp[i] += dp[i - j];
        }
    }
    return dp[n];
}
```



### Leetcode 746. Min Cost Climbing Stairs
> You are given an integer array cost where cost[i] is the cost of ith step on a staircase.
> Once you pay the cost, you can either climb one or two steps.
>
> You can either start from the step with index 0, or the step with index 1.
>
> Return the minimum cost to reach the top of the floor.


```cpp
int minCostClimbingStairs(vector<int>& cost) {
    int dp0 = 0;
    int dp1 = 0;
    for (int i = 2; i <= cost.size(); i++) {
        int dpi = min(dp1 + cost[i - 1], dp0 + cost[i - 2]);
        dp0 = dp1; // 记录一下前两位
        dp1 = dpi;
    }
    return dp1;
}
```
