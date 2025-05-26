## Two places to prune loops
Leetcode 216. Combination Sum III

> Find all valid combinations of k numbers that sum up to n such that the following conditions are true:
>
> Only numbers 1 through 9 are used.
> Each number is used at most once.
> Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.


### Pruning at the next loop
```cpp
void backtracking(int targetSum, int k, int sum, int startIndex) {
    // Pruning 1: If current sum already exceeds target sum
    if (sum > targetSum) { 
        return; 
    }
    // Base case for path length:
    if (path.size() == k) {
        if (sum == targetSum) result.push_back(path); // Check for success
        return; // Stop: path has k elements, no need to go deeper
    }

    // Pruning 2: Optimized loop bound
    for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) { 
        sum += i; 
        path.push_back(i); 
        backtracking(targetSum, k, sum, i + 1);
        sum -= i; // Explicitly revert sum
        path.pop_back(); 
    }
}
```
### Pruning at the current loop
```cpp
void backtracking(int k, int n, int start_idx, int sum) {
    // Base case for success
    if(path.size() == k && sum == n) {
        res.push_back(path);
        return;
    } 

    for (int i = start_idx; i <= 9 - (k - path.size()) + 1; ++i) {
        // Pruning 1: If current sum + i would exceed target sum
        if(sum+i > n) continue; // Or `break;` might be slightly more efficient
        // Pruning 2: If path already has k elements (or more)
        // This handles the case where path.size() == k but sum != n from the previous call
        if (path.size() >= k) continue; // Or `return;` if path.size()==k

        path.push_back(i);
        backtracking(k, n, i+1, sum+i); // Pass new sum
        path.pop_back();
    }
}
```

