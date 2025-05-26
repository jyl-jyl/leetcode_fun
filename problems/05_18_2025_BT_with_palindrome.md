## Backtraking with palindrome
Leetcode 131. Palindrome Partitioning

>Given a string s, partition s such that every substring of the partition is a palindrome.
>Return all possible palindrome partitioning of s.

Core of the solution is to identify the following
- `start_idx`: the start of the substring to check for is_palindrome
- `i` (in the loop): the end of the sbstring to check for is_palindrome
- Substring(i+1, s.size(): the part of the string that needs backtracking
- Prune loops: if the current substring is not palindrome, then we don't need to continue backtracking anymore.

```cpp
// Helper function
bool isPalindrome(const string& s, int start, int end) {
    for (int i = start, j = end; i < j; ++i, --j) {
        if (s[i] != s[j]) {
            return false;
        }
    }
    return true;
}

vector<vector<string>> res;
vector<string> path;
void backtracking(const string& s, int start_idx) {
    if (start_idx >= s.size()) {
        res.push_back(path);
        return;
    }

    for (int i = start_idx; i < s.size(); ++i) {
        if (isPalindrome(s, start_idx, i)) {
            string str = s.substr(start_idx, i - start_idx + 1);
            path.push_back(std::move(str));
        } else {
            continue;
        }
        backtracking(s, i + 1);
        path.pop_back();
    }
}

vector<vector<string>> partition(string s) {
    res.clear();
    path.clear();
    backtracking(s, 0);
    return res;
}
```

## Eliminating duplicates in backtracking
Leetcode 40. Combination Sum II
> Given a collection of candidate numbers (candidates) and a target number (target),
> find all unique combinations in candidates where the candidate numbers sum to target.
> Each number in candidates may only be used once in the combination.

It is important to understand two types of duplicates
1. duplcates on the same level
2. duplicates across different levels

In this problem, the first is allowed while the second isn't

```cpp
vector<int> path;
vector<vector<int>> res;
void backtracking(vector<int>& candidates, int target, int sum,
                  int start_idx) {
    if (sum == target) {
        res.push_back(path);
        return;
    }

    for (int i = start_idx; i < candidates.size(); ++i) {
        // remove dup on the same level but not the next level
        if (i > start_idx && candidates[i] == candidates[i - 1]) {
            continue;
        }

        if ((sum + candidates[i]) > target) {
            continue;
        }
        path.push_back(candidates[i]);
        backtracking(candidates, target, sum + candidates[i], i+1);
        path.pop_back();
    }
}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    backtracking(candidates, target, 0, 0);
    return res;
}
```








