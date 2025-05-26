## Permutations - eliminating dups across all levels
Leetcode 46. Permutations
> Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

There are two types of dups that we don't want 
1. Dups at the same level
  - Sort the array and use `if (i > start_idx && nums[i] == nums[i-1]`
  - use a `used` array to keep track of dup elements
3. Dups across all levels (in permutations, we don't want to use duplicate elements when going deep into the stack)
  - Use a `used` array to keep track of duplciates on the same branch

```cpp
vector<vector<int>> res;
vector<int> path;
// need to pass in the used array instead
void backtracking(const vector<int>& nums, vector<bool>& used)
{
    if(path.size() == nums.size())
    {
        res.push_back(path);
        return;
    }
    for (int i = 0; i < nums.size(); ++i)
    {
        if(used[i] == true) continue;
        used[i] = true;
        path.push_back(nums[i]);
        backtracking(nums, used);
        path.pop_back();
        used[i] = false; // make sure to undo
    }
}
vector<vector<int>> permute(vector<int>& nums) {
    vector<bool> used(nums.size(), false);
    backtracking(nums, used);
    return res;
}
```

Combining the two type of duplicates

Leetcode Permutations II
> Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

```cpp
vector<vector<int>> res;
vector<int> path;
void backtracking(const vector<int>& nums, vector<bool>& used)
{
    if(path.size() == nums.size())
    {
        res.push_back(path);
        return;
    }

    for (int i = 0; i < nums.size(); ++i)
    {
        // dup at the current level
        if(i > 0 && nums[i] == nums[i-1] && used[i-1] == false) continue;
        // dup across levels
        if(used[i] == true) continue;
        used[i] = true;
        path.push_back(nums[i]);
        backtracking(nums, used);
        path.pop_back();
        used[i] = false;
    }
}

vector<vector<int>> permuteUnique(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<bool> used(nums.size(), false);
    backtracking(nums, used);
    return res;
}
```
