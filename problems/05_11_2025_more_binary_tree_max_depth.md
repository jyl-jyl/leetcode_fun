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
    stack<pair<TreeNode* int>> st;
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
