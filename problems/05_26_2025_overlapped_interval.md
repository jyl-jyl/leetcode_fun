## Use greedy to solve overlapped interval problem
Leetcode 452. Minimum Number of Arrows to Burst Balloons

> There are some spherical balloons taped onto a flat wall that represents the XY-plane.
> The balloons are represented as a 2D integer array points where points[i] = [xstart, xend]
> denotes a balloon whose horizontal diameter stretches between xstart and xend.
> You do not know the exact y-coordinates of the balloons.
>
> Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis.
> A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend. There is no limit to
> the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.
>
> Given the array points, return the minimum number of arrows that must be shot to burst all balloons.

Overlap - intervals' intersection
```cpp
static bool cmp(const vector<int>& a, const vector<int>& b)
{
	return a[0] < b[0];
}

int findMinArrowShots(vector<vector<int>>& points)
{
	if(points.size() == 0) return 0;
	sort(points.begin(), points.end(), cmp);
	int res = 1;
	for (int i = 1; i < points.size(); ++i)
	{
		if(points[i][0] > points[i-1][1])
		{
			res++;
		}
		else
		{
			points[i][1] = min(points[i-1][1], points[i][1]); // take intersection on the right boundary
		}
	}
	return res;
}
```

Leetcode 435. Non-overlapping Intervals is similar
> Given an array of intervals intervals where intervals[i] = [starti, endi],
> return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
>
> Note that intervals which only touch at a point are non-overlapping. For example, [1, 2] and [2, 3] are non-overlapping.


```cpp
static bool cmp(const vector<int>& v1, const vector<int>& v2) {
    return v1[0] < v2[0];
}

int eraseOverlapIntervals(vector<vector<int>>& intervals) {
	sort(intervals.begin(), intervals.end(), cmp);
	int count = 0;
	auto size = intervals.size();
	for (int i = 1; i < size; ++i)
	{
		auto& curr = intervals[i];
		const auto& prev = intervals[i-1];
		if(curr[0] < prev[1])
		{
			count ++;
			curr[1] = min(curr[1], prev[1]);
		}

	}
	return count;
}
```

Leetcode 763. Partition Labels

> You are given a string s. We want to partition the string into as many parts as possible so that each letter
> appears in at most one part. For example, the string "ababcc" can be partitioned into ["abab", "cc"], but partitions
> such as ["aba", "bcc"] or ["ab", "ab", "cc"] are invalid.
>
> Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.
>
> Return a list of integers representing the size of these parts.

This problem has two interesting points
1. This problem requires the union of intervals but not the intersections
2. This problem can be seen similar to the jump game II problem. The next jump check `i == curr_max_reach`

```cpp
vector<int> partitionLabels(string S) {
	int hash[27] = {0};
	for (int i = 0; i < S.size(); ++i)
	{
		hash[S[i] - 'a'] = i; 
	}

	vector<int> res;
	int left = 0;
	int right = 0;
	for (int i = 0; i < S.size(); ++i)
	{
		right = max(right, hash[S[i] - 'a']);
		if(i == right)
		{
			res.push_back(right-left+1);
			left = i+1;
		}
	}
	return res;
}
```
