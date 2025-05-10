## Three ways of searching a binary tree
1. DFS
    1. Recursive
    2. Iterative (using stack)
2. BFS (uses queue)

Take an example of Leetcode 226 Invert Binary Tree
>Given the root of a binary tree, invert the tree, and return its root.

Two traverse orders work for this problem
1. Preorder traversal
2. Postorder traversal

I'm going to try to provide solutions for each one of the orders and in each of the ways (almost), so there will be 6 solutions in total

### Preorder
1. Recursive DFS
```cpp
TreeNode* invertTree(TreeNode* root)
{
  if(!root) return root;
  swap(root->left, root->right);
  invertTree(root->left);
  invertTree(root->right);
  return root;
}
```
2. Iterative DFS
```cpp
TreeNode* invertTree(TreeNode* root)
{
  stack<TreeNode*> st;
  if(!root) return root;
  st.push(root);
  while(!st.empty())
  {
    auto node = st.top();
    if(node)
    {
        st.pop();
        if(node->right) st.push(node->right);
        if(node->left) st.push(node->left);
        st.push(node);
        st.push(nullptr);
    }
    else
    {
        st.pop();
        auto node = st.top();
        st.pop();
        swap(node->right, node->left);
    }
  }
  return root;
}
```

### Postorder
1. Recursive DFS
 ```cpp
TreeNode* invertTree(TreeNode* root)
{
  if(!root) return root;
  invertTree(root->left);
  invertTree(root->right);
  swap(root->left, root->right);
  return root;
}
```
2. Iterative DFS
```cpp
TreeNode* invertTree(TreeNode* root)
{
  stack<TreeNode*> st;
  if(!root) return root;
  st.push(root);
  while(!st.empty())
  {
    auto node = st.top();
    if(node)
    {
        st.pop();
        st.push(node);
        st.push(nullptr);
        if(node->right) st.push(node->right);
        if(node->left) st.push(node->left);
    }
    else
    {
        st.pop();
        auto node = st.top();
        st.pop();
        swap(node->right, node->left);
    }
  }
  return root;
}
```

### Inorder
1. Iterative
```cpp
TreeNode* invertTree(TreeNode* root)
{
  stack<TreeNode*> st;
  if(!root) return root;
  st.push(root);
  while(!st.empty())
  {
    auto node = st.top();
    if(node)
    {
        st.pop();
        if(node->right) st.push(node->right);
        st.push(node);
        st.push(nullptr);
        if(node->left) st.push(node->left);
    }
    else
    {
        st.pop();
        auto node = st.top();
        st.pop();
        swap(node->right, node->left);
    }
  }
  return root;
}
```

### BFS
```cpp
TreeNode* invertTree(TreeNode* root)
{
  queue<TreeNode*> que;
  if(!root) que.push(root);
  while(!que.empty())
  {
    auto size = que.size();
    while(size--)
    {
        auto node = que.front();
        que.pop();
        swap(node->left, node->right);
        if(node->left) que.push(node->left);
        if(node->right) que.push(node->right);
    }
  }
  return root;
}
```
