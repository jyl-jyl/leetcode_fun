## More solutions to the LCA of a binary tree
Leetcode 236 Lowest Common Ancestor of a Binary Tree

### Recursive postorder
```cpp
TreeNode* lca(TreeNode* root, TreeNode* p, TreeNOde* q)
{
  if(root == q || root == p || !root) return root;
  auto left = lca(root->left, p, q);
  auto right = lca(root->right, p, q);
  if(left && right) return root;
  if(left && !right) return left;
  if(!left && right) return right;
  else return nullptr;
}
```

### Iterative modified postorder 
```cpp
TreeNode* lca(TreeNode* root, TreeNode* p, TreeNode* q)
{
  if(!root) return nullptr;
  vector<TreeNode*> path_p, path_q;
  stack<TreeNode*> st;
  TreeNode* prev = nullptr;
  TreeNode* curr = root;

  auto findPath = [&}(TreeNode* target, vector<TreeNode*>& path)
  {
    st = {};
    prev = nullptr;
    curr = root;
    while(curr || !st.empty())
    {
      while(curr)
      {
        st.push(curr);
        curr = curr->left;
      }
      curr = st.top();
      if(!curr->right || curr->right == prev)
      {
        if(curr == target)
        {
          while(st.empty())
          {
            path.push_back(st.top())
            st.pop();
          }
          std::reverse(path.begin(), path.end());
          return true;
        }
        st.pop();
        prev = curr;
        curr = nullptr;
      }
      else
      {
        curr = curr->right;
      }
    }
    return false;
  };
  findPath(p, path_p);
  findPath(q, path_q);
  int min_len = min(path_p.size(), path_q.size());
  TreeNode* lca = nullptr;
  for (int i = 0; i < min_len; ++i)
  {
    if(path_p[i] == path_q[i]) lca = path_p[i];
    else break;
  }
  return lca;
}
```

### BFS (sort of like Union Find)
```cpp
TreeNode* lca(TreeNode* root, Treenode* p, TreeNode* p)
{
  if(!root) return nullptr;
  unordered_map<TreeNode*, TreeNode*> parent;
  queue<TreeNode*> q_nodes;
  parent[root] = nullptr;
  q_nodes.push(root);
  while(!parent.count(p) || !parent.count(q))
  {
    auto node = q_nodes.front();
    q_nodes.pop();
    if(node->left) 
    {
      parent[node->left] = node;
      q_nodes.push(node->left);
    }

    if(node->right) 
    {
      parent[node->right] = node;
      q_nodes.push(node->right);
    }
  }

  unordered_set<TreeNode*> ancestors;
  while(p)
  {
    ancestors.insert(p);
    p = parent[p];
  }
  while(!ancestors.count(q))
  {
    q = parent[q];
  }
  return q;
}
```

### Performance analysis
- Recursive postorder
  - Time: O(n) — Each node is visited once.
  - Space: O(h) — Call stack size, where h is the height of the tree.
    - Worst case: O(n) for skewed trees.
    - Best/average (balanced): O(log n).
  - Performance
    - Fastest in practice for most LeetCode cases.
    - No overhead of data structures.
    - Tail recursion optimization (if supported by compiler) helps. 
- Iterative modified postorder
  - Time: O(n) — In the worst case, you visit all nodes.
  - Space: O(n) — Stack to store nodes and paths.
  - Performance:
    - Stack management and manual state tracking (prev, etc.).
    - Extra cost of storing and reversing paths.
    - Comparing two vectors to find LCA adds overhead.
- BFS
  - Time: O(n) — Each node is visited once.
  - Space: O(n) — For the parent map and ancestor set.
  - Performance
    - Faster than iterative DFS, especially for shallow trees.
    - Slightly slower than recursive DFS for small trees but can outperform recursive in extremely deep trees due to no recursion stack.
   
- Leetcode result (from top to bottom are BFS, Iterative DFS, and Recursive)
<img width="472" alt="Screenshot 2025-05-15 at 12 16 05 AM" src="https://github.com/user-attachments/assets/9429b963-d2a6-41e0-acec-86c7e58d7ab7" />
