## More BST inorder traveral 
Leetcode 530. Minimum Absolute Difference in BST
> Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

### Recursive inorder
```cpp
int result = INT_MAX;
TreeNode* pre = NULL;
void traversal(TreeNode* cur) {
    if (cur == NULL) return;
    traversal(cur->left);   // 左
    if (pre != NULL){       // 中
        result = min(result, cur->val - pre->val);
    }
    pre = cur; // 记录前一个
    traversal(cur->right);  // 右
}

int getMinimumDifference(TreeNode* root) {
    traversal(root);
    return result;
}
```

### Iterative inorder
```cpp
int getMinimumDifference(TreeNode* root) {
    stack<TreeNode*> st;
    TreeNode* cur = root;
    TreeNode* pre = NULL;
    int result = INT_MAX;
    while (cur != NULL || !st.empty()) {
        if (cur != NULL) { 
            st.push(cur); 
            cur = cur->left;                
        } else {
            cur = st.top();
            st.pop();
            if (pre != NULL) {             
                result = min(result, cur->val - pre->val);
            }
            pre = cur;
            cur = cur->right;             
        }
    }
    return result;
}
```
