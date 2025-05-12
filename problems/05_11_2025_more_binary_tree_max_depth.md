## More problems around the max depth of a binary tree
Leetcode 513 Find Bottom Left Tree Value
>Given the root of a binary tree, return the leftmost value in the last row of the tree.

Recursive
```cpp
int maxDepth = INT_MIN;
int result;
void traversal(TreeNode* root, int depth) {
    if (root->left == NULL && root->right == NULL) {
        if (depth > maxDepth) {
            maxDepth = depth;
            result = root->val;
        }
        return;
    }
    if (root->left) {
        depth++;
        traversal(root->left, depth);
        depth--; // 回溯
    }
    if (root->right) {
        depth++;
        traversal(root->right, depth);
        depth--; // 回溯
    }
    return;
}
int findBottomLeftValue(TreeNode* root) {
    traversal(root, 0);
    return result;
}
```
Iterative Postorder
```cpp
 int findBottomLeftValue(TreeNode* root) {
    int res = 0;
    if (!root)
        return 0;
    stack<TreeNode*> st;
    st.push(root);
    int depth = 0;
    int max_depth = INT_MIN;
    while (!st.empty()) {
        auto node = st.top();
        if (node) {
            st.pop();
            st.push(node);
            st.push(nullptr);
            depth++;
            if (depth > max_depth) {
                max_depth = depth;
                res = node->val;
            }
            if (auto right = node->right)
                st.push(right);
            if (auto left = node->left)
                st.push(left);

        } else {
            st.pop();
            st.pop();
            depth--;
        }
    }
    return res;
}
```
We can also get rid of the marker (for nullptr) by using an explicit stack
```cpp
int findBottomLeftValue(TreeNode* root)
{
    int res = 0;
    int max_depth = -1;
    stack<pair<TreeNode*, int>> st;
    st.push({root, 0});
    while(!st.empty())
    {
        auto [node, depth] = st.top();
        st.pop();
        if(node)
        {
            if(depth > max_depth)
            {
                max_depth = depth;
                res = node->val;
            {
            if(auto right = node->right) st.push({right, depth+1});
            if(auto left = node->left) st.push({left, depth+1});
        }
    }
    return res;
}
```

BFS
```cpp
int findBottomLeftValue(TreeNode* root) {
    queue<TreeNode*> que;
    if (root != NULL) que.push(root);
    int result = 0;
    while (!que.empty()) {
        int size = que.size();
        for (int i = 0; i < size; i++) {
            TreeNode* node = que.front();
            que.pop();
            if (i == 0) result = node->val; // 记录最后一行第一个元素
            if (node->left) que.push(node->left);
            if (node->right) que.push(node->right);
        }
    }
    return result;
}
```

## Another exmaple of using both recursive and iterative postorder to get the path sum
Leetcode 113 Path Sum II
> Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where the sum of the node values in the path equals targetSum. Each path should be returned as a list of the node values, not node references.
>
>A root-to-leaf path is a path starting from the root and ending at any leaf node. A leaf is a node with no children.

The iterative solution does not need to marker nullptr and follow the pop + `depth--` logic because all current sum and current vector is stored in separate stacks. This approach is similar to using a explicit stack above

Recursive
```cpp
void traversal(TreeNode* cur, int count, vector<int>& path, vector<vector<int>>& res) {
    if (!cur->left && !cur->right && count == 0) {
        res.push_back(path);
        return; 
    }
    if (!cur->left && !cur->right)
        return; 

    if (auto left = cur->left) {
        path.push_back(left->val); 
        traversal(cur->left, count-left->val, path, res);
        path.pop_back(); 
    }
    if (auto right = cur->right) {
        path.push_back(right->val); 
        traversal(cur->right, count-right->val, path, res);
        path.pop_back(); 
    }
    return;
}

vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
    vector<vector<int>> res{};
    if (root == NULL)
        return res;
    vector<int> path;
    path.push_back(root->val);
    traversal(root, targetSum - root->val, path, res);
    return res;
}
```

Iterative postorder
```cpp
vector<vector<int>> pathSum(TreeNode* root, int sum) {
    vector<vector<int>> res;
    if (!root)
        return res;

    stack<TreeNode*> st;
    stack<vector<int>> pathSt;
    stack<int> sumSt;

    st.push(root);
    pathSt.push({root->val});
    sumSt.push(root->val);

    while (!st.empty()) {
        TreeNode* node = st.top();
        st.pop();
        vector<int> path = pathSt.top();
        pathSt.pop();
        int currSum = sumSt.top();
        sumSt.pop();

        // If it's a leaf node and the sum matches, record the path
        if (!node->left && !node->right && currSum == sum) {
            res.push_back(path);
        }

        // Push right child
        if (node->right) {
            st.push(node->right);
            vector<int> newPath = path;
            newPath.push_back(node->right->val);
            pathSt.push(newPath);
            sumSt.push(currSum + node->right->val);
        }

        // Push left child
        if (node->left) {
            st.push(node->left);
            vector<int> newPath = path;
            newPath.push_back(node->left->val);
            pathSt.push(newPath);
            sumSt.push(currSum + node->left->val);
        }
    }

    return res;
}
```

