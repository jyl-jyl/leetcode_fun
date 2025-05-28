## An very intersting problem of Greedy + Binary Tree
Leetcode 968. Binary Tree Cameras

> You are given the root of a binary tree. We install cameras on the tree nodes where each
> camera at a node can monitor its parent, itself, and its immediate children.
> Return the minimum number of cameras needed to monitor all nodes of the tree.

1. Traverse from leaf nodes - Postorder
2. Define status by ourselves:
  - Monitored
  - Not monitored
  - Camera
3. Pratice recursive and iterative method for traversing a binary tree

### Postorder traverse
```cpp
enum Status { NOT_MONITORED = 0, MONITORED = 1, CAMERA = 2 };

int traverse(TreeNode* node, int& cameras) {
    if (!node) return MONITORED;

    int left = traverse(node->left, cameras);
    int right = traverse(node->right, cameras);

    if (left == NOT_MONITORED || right == NOT_MONITORED) {
        cameras++;
        return CAMERA;
    }

    if (left == CAMERA || right == CAMERA) {
        return MONITORED;
    }

    return NOT_MONITORED;
}

int minCameraCover(TreeNode* root) {
    int cameras = 0;
    if (traverse(root, cameras) == NOT_MONITORED) {
        cameras++; // place camera at root
    }
    return cameras;
}
```

### Postorder iterative
```cpp
enum Status { NOT_MONITORED = 0, MONITORED = 1, CAMERA = 2 };

int minCameraCover(TreeNode* root) {
    if (!root)
        return 0;

    unordered_map<TreeNode*, int> state;
    stack<TreeNode*> stk;
    TreeNode* cur = root;
    TreeNode* prev = nullptr;
    int cameras = 0;

    while (!stk.empty() || cur) {
        while (cur) {
            stk.push(cur);
            cur = cur->left;
        }

        cur = stk.top();
        if (!cur->right || cur->right == prev) {
            stk.pop();

            int left = cur->left ? state[cur->left] : MONITORED;
            int right = cur->right ? state[cur->right] : MONITORED;

            if (left == NOT_MONITORED || right == NOT_MONITORED) {
                state[cur] = CAMERA;
                ++cameras;
            } else if (left == CAMERA || right == CAMERA) {
                state[cur] = MONITORED;
            } else {
                state[cur] = NOT_MONITORED;
            }

            prev = cur;
            cur = nullptr;
        } else {
            cur = cur->right;
        }
    }

    if (state[root] == NOT_MONITORED) {
        ++cameras;
    }

    return cameras;
}
```


