## Two-pass greedy
Leetcode 135. Candy
> There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.
>
> You are giving candies to these children subjected to the following requirements:
>
> Each child must have at least one candy.
> Children with a higher rating get more candies than their neighbors.
> Return the minimum number of candies you need to have to distribute the candies to the children.

- Pass_1: Make sure right->left has the right candy (0 -> size-1)
- Pass_2: Make sure the left->right has the right candy (size-1 -> 0)

```cpp
int candy(vector<int>& ratings) {
    auto size = ratings.size();
    vector<int> res(size, 1);
    for (int i = 1; i < size; ++i) {
        if (ratings[i] > ratings[i - 1]) {
            res[i] = res[i - 1] + 1;
        }
    }

    for (int i = size - 1; i >= 1; --i) {
        if (ratings[i - 1] > ratings[i]) {
            res[i - 1] = max(res[i] + 1, res[i - 1]);
        }
    }
    return accumulate(res.begin(), res.end(), 0);
}
```

Leetcode 406. Queue Reconstruction by Height

>You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order).
>Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a
>height greater than or equal to hi.
>
>Reconstruct and return the queue that is represented by the input array people.
>The returned queue should be formatted as an array queue, where queue[j] = [hj, kj]
>is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

- Pass_1: sort the array by `h`
- Pass_2: reorder the array by `k`

```cpp
static bool cmp(const vector<int>& p1, const vector<int>& p2) {
    if (p1[0] == p2[0]) {
        return p1[1] < p2[1];
    }
    return p1[0] > p2[0];
}

vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    sort(people.begin(), people.end(), cmp); // pass_1
    list<vector<int>> res;
    for (const auto& p : people) { // pass_2
        int pos = p[1];
        auto it = res.begin();
        while (pos--) {
            it++;
        }
        res.insert(it, p);
    }
    return vector<vector<int>>(res.begin(), res.end());
}
```
