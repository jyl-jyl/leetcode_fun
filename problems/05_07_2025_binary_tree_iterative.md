## Iterative Traversal of Binary Trees 
### Preorder  (MID - LEFT - RIGHT)
Add to result vector when pushing into stack
```cpp
vector<int> preorderTraversal(TreeNode* root)
{
	vector<int> res;
	stack<TreeNode*> st;

  // Add the leftmost branch to stack
	while(root)
	{
	  res.push_back(root->val);
	  st.push(root);
	  root = root->left;
	}
  
	// Then check if the top of the stack has right
  // If it has right, then add all its left nodes to stack
	while(!st.empty())
	{
		TreeNode* cur = st.front();
		st.pop();
		TreeNode* right = cur->right;
		while(right)
		{
      res.push_back(right->val);
			st.push(right);
			right = right->left;
		}
	}
	return res;
}
```

### Inorder   (LEFT - MID - RIGHT)
Add to result vector when popping out of stack
```cpp
vector<int> inorderTraversal(TreeNode* root)
{
	vector<int> res;
	stack<TreeNode*> st;

	// Add the leftmost branch to stack
	while(root)
	{
	  st.push(root);
	  root = root->left;
	}

	// Then check if the top of the stack has right
  // If it has right, then add all its left node to stack
	while(!st.empty())
	{
		TreeNode* cur = st.front();
		st.pop();
		res.push_back(cur->val);
		TreeNode* right = cur->right;
		while(right)
		{
			st.push(right);
			right = right->left;
		}
	}
	return res;
}
```

### Postorder (LEFT - RIGHT - MID) slightly different
Postorder is reverse Preorder then swap LEFT and RIGHT, also add to result vector when popping out of stack
```cpp
vector<int> postorderTraversal(TreeNode* root)
{
	vector<int> res;
	stack<TreeNode*> st;

	// Add the rightmost branch to stack
	while(root)
	{
	  res.push_back(root->val);
	  st.push(root);
	  root = root->right;
	}

	// Then check if the top of the stack has left
  // If it has left, then add all its right nodes to stack
	while(!st.empty())
	{
		TreeNode* cur = st.front();
		st.pop();
		TreeNode* left = cur->left;
		while(left)
		{
		    res.push_back(left->val);
			st.push(left);
			left = left->right;
		}
	}

	// Reverse the vector
	for (int i = 0, j = res.size()-1; i < res.size()/2; ++i, --j)
	{
		swap(res[i], res[j]);
	}

	return res;
}
```
