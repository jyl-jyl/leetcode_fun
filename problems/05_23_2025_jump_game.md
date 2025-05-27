## Jump game - when are we stuck?
55. Jump Game

> You are given an integer array nums. You are initially positioned at the array's first index,
> and each element in the array represents your maximum jump length at that position.
>
> Return true if you can reach the last index, or false otherwise.

### More intuitive solution
We keep track of the max reach, if it is greater than the last index, then return true
```cpp
bool canJump(vector<int>& nums) {
    int cover = 0;
    if (nums.size() == 1) return true;

    for (int i = 0; i <= max_reach; i++) { // 注意这里是小于等于cover
        max_reach = max(i + nums[i], max_reach);
        if (max_reach >= nums.size() - 1) return true; // 说明可以覆盖到终点了
    }
    return false;
}
```

### Looking for the point when we are stuck
We are only stuck when this happen:
> i > max_reach

So we can use that to
```cpp
bool max_reach(vector<int>& nums) {
    int maxReach = 0;
    for (int i = 0; i < nums.size(); ++i) {
        if (i > max_reach) 
            return false; 
        max_reach = max(max_reach, i + nums[i]);
    }
    return true;
}
```

## Sorting supporting Greedy
Leetcode 1005. Maximize Sum Of Array After K Negations
>Given an integer array nums and an integer k, modify the array in the following way:
>
>choose an index i and replace nums[i] with -nums[i].
>You should apply this process exactly k times. You may choose the same index i multiple times.
>Return the largest possible sum of the array after modifying it in this way.

This problem is interesting because the greedy solution relies on an uncommon way of sorting the array - 
putting number with the maximum abs value at the two ends of the array.

`-3`, `-2`, `-1`, `3`, `2`, `1`

```cpp
int largestSumAfterKNegations(vector<int>& nums, int k) {
    sort(nums.begin(), nums.end(), cmp);
    for (int i = 0; i < nums.size(); ++i)
    {
        if (nums[i] < 0 && k > 0)
        {
            nums[i] *= -1;
            k--;
        }
    }
    if (k % 2 == 1)
    {
        nums[nums.size()-1] *= -1;
    }
    int res = 0;
    for (int n : nums) res += n;
    return res;  
}
```


