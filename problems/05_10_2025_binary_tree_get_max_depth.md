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

## For iterative solutions, we don't have to use marker all the time!
Leetcode 257 Binary Tree Path
```cpp
vector<string> binaryTreePaths(TreeNode* root) {
    stack<TreeNode*> treeSt;// 保存树的遍历节点
    stack<string> pathSt;   // 保存遍历路径的节点
    vector<string> result;  // 保存最终路径集合
    if (root == NULL) return result;
    treeSt.push(root);
    pathSt.push(to_string(root->val));
    while (!treeSt.empty()) {
        TreeNode* node = treeSt.top(); treeSt.pop(); // 取出节点 中
        string path = pathSt.top();pathSt.pop();    // 取出该节点对应的路径
        if (node->left == NULL && node->right == NULL) { // 遇到叶子节点
            result.push_back(path);
        }
        if (node->right) { // 右
            treeSt.push(node->right);
            pathSt.push(path + "->" + to_string(node->right->val));
        }
        if (node->left) { // 左
            treeSt.push(node->left);
            pathSt.push(path + "->" + to_string(node->left->val));
        }
    }
    return result;
}
```
### The purpose of the nullptr markers
In iterative postorder or inorder traversals with a single stack, we usually use nullptr markers to simulate the recursive call stack and ensure a node is processed only after its children. That’s especially important when:
- You need to visit a node after both its left and right children have been processed (e.g., for postorder).
- You are tracking depth or need to update a result during backtracking.

The solution for binary tree paths, the traversal logic is fundamentally `preorder`, not `postorder`. This is because when we add in a node, we pop it out and process it immediately 

### Key reasons why this works without nullptr
1. You process the current node immediately:
Each time you pop a node from the stack, you already know the full path up to that node (kept in pathSt).
You don’t need to "return to" this node after processing its children (which is why markers are usually used).
2. You don't care about backtracking:
You're not calculating anything based on "after visiting left and right."
Once you reach a leaf node, you output the current accumulated path and move on.
3. Separate stacks keep state:
You keep a treeSt for nodes and a parallel pathSt for the paths leading to those nodes.
This eliminates the need for simulating the call stack using nullptr markers.

**When you only need to traverse all nodes and process each node as you traverse, using preorder iterative can also be a simple approach**
leetcode 222 Count Complete Tree Nodes
```cpp
int countNodes(TreeNode* root) {
    stack<TreeNode*> st;
    if (!root)
        return 0;
    st.push(root);
    int res = 0;
    while (!st.empty()) {
        auto node = st.top();
        st.pop();
        res++;
        if (auto right = node->right)
            st.push(right);
        if (auto left = node->left)
            st.push(left);
    }
    return res;
}
```





