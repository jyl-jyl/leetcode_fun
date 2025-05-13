## Operations on 2 binary trees - same as one tree
Leetcode 617 Merge Two Binary Trees

> You are given two binary trees root1 and root2.
> 
> Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.
>
> Return the merged tree.
>
> Note: The merging process must start from the root nodes of both trees.

### Recursive
```cpp
TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
    if (t1 == NULL) return t2; 
    if (t2 == NULL) return t1; 
    t1->val += t2->val;                             
    t1->left = mergeTrees(t1->left, t2->left);      
    t1->right = mergeTrees(t1->right, t2->right);  
    return t1;
}
```

### Iterative Preorder
```cpp
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
    if (!root1)
        return root2;
    if (!root2)
        return root1;

    stack<TreeNode*> st1;
    stack<TreeNode*> st2;
    st1.push(root1);
    st2.push(root2);

    while (!st1.empty() || !st2.empty()) {
        auto node_1 = st1.top();
        st1.pop();
        auto node_2 = st2.top();
        st2.pop();

        node_1->val += node_2->val;

        auto left_1 = node_1->left;
        auto right_1 = node_1->right;

        auto left_2 = node_2->left;
        auto right_2 = node_2->right;

        if (right_1 && right_2) {
            st1.push(right_1);
            st2.push(right_2);
        }

        if (!right_1 && right_2) {
            node_1->right = node_2->right;
        }

        if (left_1 && left_2) {
            st1.push(left_1);
            st2.push(left_2);
        }

        if (!left_1 && left_2) {
            node_1->left = node_2->left;
        }
    }
    return root1;
}
```

### BFS
```cpp
TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
    if (t1 == NULL) return t2;
    if (t2 == NULL) return t1;
    queue<TreeNode*> que;
    que.push(t1);
    que.push(t2);
    while(!que.empty()) {
        TreeNode* node1 = que.front(); que.pop();
        TreeNode* node2 = que.front(); que.pop();
        node1->val += node2->val;

        if (node1->left != NULL && node2->left != NULL) {
            que.push(node1->left);
            que.push(node2->left);
        }
        if (node1->right != NULL && node2->right != NULL) {
            que.push(node1->right);
            que.push(node2->right);
        }

        if (node1->left == NULL && node2->left != NULL) {
            node1->left = node2->left;
        }

        if (node1->right == NULL && node2->right != NULL) {
            node1->right = node2->right;
        }
    }
    return t1;
}
```
