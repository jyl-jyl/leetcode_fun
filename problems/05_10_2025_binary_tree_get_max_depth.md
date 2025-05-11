## Get the max depth of a binary tree (or a certain node in a binary tree)
Leetcode 104

### Postorder
- Recursive (not so different with preorder recursive)
```cpp
{
  if(!root) return 0;
  return max(maxDepth(root->left), maxDepth(root->right)) + 1;
}
```
- Iterative（向上回溯的时候处理节点，但是需要在向下遍历的时候增加深度，回溯的时候还原深度）
```cpp
int getDepth(TreeNode* cur)
{
  stack<TreeNode*> st;
  if(curr) st.push(curr);
  int depth = 0;
  int res = 0;
  while(!st.empty())
  {
    auto node = st.top();
    if(node)
    {
      st.pop();
      st.push(node);
      st.push(nullptr);
      depth++; // 每向下遍历一层，depth+1
      res = max(res, depth);
      if(auto left = node->left) st.push(left);
      if(auto right = node->right) st.push(right);
    }
    else
    {
      st.pop();
      auto node = st.top();
      st.pop();
      depth--; // 每回溯一层，depth-1
    }
  }
  return res;
}
```

### Preorder
- Recursive
```cpp
int maxDepth(TreeNode* root)
{
  if(!root) return 0;
  return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
```
- Iterative- DOES NOT WORK
  - Preorder stack: right -> left -> node -> null
  - When we add the node + null, we depth++
  - We will pop it out right after, then we depth--
  - Then we will process right and left node, which is already added and will not change the depth
  - Therefore, the depth is always 1
  
### Inorder
- Recusive - DOES NOT WORK
The depth at a node depends on the max depth of its left and right subtrees,
which is something inorder traversal doesn't naturally help with,
since it splits visiting left and right around visiting the current node.

- Iterative - DOES NOT WORK
  - Inorder stack; right -> node -> null -> left
  - As long as we're progressing with the left most branch, we are correctly calculating the depth (depth++)
  - However, if we reach a node that doesn't have a left node
  - The stack looks like: right -> node -> null
  - We will start to pop the node out, which will depth--
  - Then we will process the pre-added right node, which will not change the depth
  - Therefore, the depth will be the longest leftmost branch of the binary tree
  
### BFS
```cpp
int getDepth(TreeNode* curr)
{
  if(!curr) return 0;
  int depth = 0;
  queue<TreeNode*> que;
  que.push(curr);
  while(!que.empty())
  {
    auto size = que.size();
    depth++;
    while(size--)
    {
      auto node = que.front();
      que.pop();
      if(auto left = node->left) que.push(left);
      if(auto right = node->right) que.push(right);
    }
  }
  return depth;
}
```







