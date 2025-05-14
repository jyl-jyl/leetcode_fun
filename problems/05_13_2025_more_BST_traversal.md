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

Leetcode 501. Find Mode in Binary Search Tree
> Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred
> element) in it.
>
> If the tree has more than one mode, return them in any order.
>
> Assume a BST is defined as follows:
>
> The left subtree of a node contains only nodes with keys less than or equal to the node's key.
> The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
> Both the left and right subtrees must also be binary search trees.

### Recursive inorder
```cpp
int maxCount = 0; 
int count = 0; 
TreeNode* pre = NULL;
vector<int> result;
void searchBST(TreeNode* cur) {
    if (cur == NULL) return ;

    searchBST(cur->left);       
                                
    if (pre == NULL) { 
        count = 1;
    } else if (pre->val == cur->val) { 
        count++;
    } else { 
        count = 1;
    }
    pre = cur;

    if (count == maxCount) { 
        result.push_back(cur->val);
    }

    if (count > maxCount) { 
        maxCount = count;   
        result.clear();     
        result.push_back(cur->val);
    }

    searchBST(cur->right);      
    return ;
}

vector<int> findMode(TreeNode* root) {
    count = 0;
    maxCount = 0;
    pre = NULL; 
    result.clear();

    searchBST(root);
    return result;
}
```

### Iterative inorder
```cpp
vector<int> findMode(TreeNode* root) {
    stack<TreeNode*> st;
    TreeNode* cur = root;
    TreeNode* pre = NULL;
    int maxCount = 0; 
    int count = 0; 
    vector<int> result;
    while (cur != NULL || !st.empty()) {
        if (cur != NULL) { 
            st.push(cur); 
            cur = cur->left;                
        } else {
            cur = st.top();
            st.pop();                       
            if (pre == NULL) {
                count = 1;
            } else if (pre->val == cur->val) { 
                count++;
            } else { 
                count = 1;
            }
            if (count == maxCount) { 
                result.push_back(cur->val);
            }

            if (count > maxCount) { 
                maxCount = count;   
                result.clear();     
                result.push_back(cur->val);
            }
            pre = cur;
            cur = cur->right;              
        }
    }
    return result;
}
```

