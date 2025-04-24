## Starting problem
Leetcode 704  
```
int search(vector<int>& nums, int target)
{
   int left = 0;
   int right = nums.size()-1;

   while(left <= right)
   {
      auto mid = (left+right)/2;
      if(nums[mid] > target)
      {
          right = mid-1;
      }
      else if(nums[mid] < target)
      {
          left = mid+1;
      }
      else
      {
          return mid;
      }
   }
   return -1;
}
```

## Follow-up 1

Leetcode 875 Koko Eating Bananas
> Koko has a bunch of banana piles. Each hour, she can choose one pile and eat up to k bananas from it (if the pile has less than k, she eats all of them). Given an integer h (total number of hours), find the minimum eating speed k such that she can eat all bananas in h hours.

Search on the range from possible **smallest possible speed (1)** to the **largest possible speed (max(piles))**

- Solution

```
bool can_finish(const vector<int>& piles, int speed, int h
{
  long long toal_hours = 0;
  for (const auto& p : piles)
  {
    total_hours += (p + 1LL*speed - 1) / speed;
  }
  return total_hours <= h;
}

int minEatingSpeed(vector<int>& piles, int h)
{
  int left = 1;
  int right = *max_element(piles.begin(), piles.end();
  while(left <= right)
  {
    int mid = (left + right) / 2;
    if (can_finish(piles, mid, h))
    {
      right = mid-1;
    }
    else
    {
      left = mid+1;
    }
  }
  return left;
}
```

## Follow-up 2

lower/upper bounds in c++ stl
- Example
```
vector<int> nums = {1, 3, 3, 5, 7};

auto lb = lower_bound(nums.begin(), nums.end(), 3); // points to first 3
auto ub = upper_bound(nums.begin(), nums.end(), 3); // points to 5

cout << lb - nums.begin(); // 1
cout << ub - nums.begin(); // 3
```
- Implementation using binary search (STL)

Note that the below implmentation works for arrays with dup elements too. 
Lower bound means we need to find the first occurance of a certain value
So what do we do when we find `target == nums[mid]`?
We only need to update right so that we can keep searching to the left
Similarly for calculating the upper bound
```
int lower_bound(const vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    while (left < right) {
        int mid = left + (right - left)/2;
        if (nums[mid] < target)
            left = mid + 1;
        else
            right = mid;
    }
    return left;
}

int upper_bound(const vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    while (left < right) {
        int mid = left + (right - left)/2;
        if (nums[mid] <= target)
            left = mid + 1;
        else
            right = mid;
    }
    return left;
}
```

## Follow-up 3 - Binary Search + Greedy (Finally)
Leetcode 410 Split Array Largest Sum
> Given an array nums and integer k, split the array into k or fewer subarrays such that the largest subarray sum is minimized.

1. Binary search the answer from the **largest subarray sum(sum(array))** to the **smallest subarray sum (max(array))**

2. Greedy check: given a value from the range above (val), ask `Can I split the array into <= k parts such that each part's sum <= val`?

3. If yes, then search update right, if no then update left
```
bool can_split(const vector<int>& nums, int max_sum, int k)
{
int count = 1;
long long curr_sum = 0;
for(const auto n : nums)
{
  if(current_sum > max_sum)
  {
    count++;
    curr_sum = n;
  }
  else
  {
    curr_sum += n;
  }
}
return count <= k;
}

int splitArray(vector<int>& nums, int k)
{
  int left = *max_element(nums,begin(), nums.end());
  int right = accumulate(nums.begin(), nums,end(), 0);
  
  while(left <= right)
  {
    auto mid = left + (right-left)/2;
    if(can_split(nums, mid, k))
    {
      right = mid-1;
    }
    else
    {
      left = mid+1;
    }
  }
  return left;
}
```
 

