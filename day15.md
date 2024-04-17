# day15

## 层序遍历（可解决10题）

层序遍历：

```cpp
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

深度优先遍历-递归-先进后出-用栈

层序遍历-一层一层遍历-先进先出-队列

**直接背 层序遍历的模板：（递归版 深度优先遍历 dfs）**

```cpp
class Solution {
public:
    void order(TreeNode* cur, vector<vector<int>>& result, int depth)
    {
        if (cur == nullptr) return;
        if (result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        order(cur->left, result, depth + 1);
        order(cur->right, result, depth + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int depth = 0;
        order(root, result, depth);
        return result;
    }
};//leetcode102原题
```

**层序遍历模板（非递归 队列 广度优先遍历 bfs）**

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        vector<vector<int>> result;
        while (!que.empty()) {
            int size = que.size();
            vector<int> vec;
            // 这里一定要使用固定大小size，不要使用que.size()，因为que.size是不断变化的
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

可以解决：leetcode102，107，199，637，429，515，116，117，104，111

自底向上的层序遍历，就是从头往下的result反转了即可。

```cpp
//自底向上的层序遍历
class Solution {
public:
    void order(TreeNode*cur,vector<vector<int>>&vec,int depth){
        if(cur==nullptr)return;
        if(vec.size()==depth){
            vec.push_back(vector<int>());
        }
        vec[depth].push_back(cur->val);
        order(cur->left,vec,depth+1);
        order(cur->right,vec,depth+1);
        //reverse(vec.begin(),vec.end());
    }
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>>result;
        int depth=0;
        order(root,result,depth);
        reverse(result.begin(),result.end());
        return result;
    }
};
```

注意：reverse函数之后，他会重写在result上，可以直接返回了。

右视图题leetcode199

```cpp
class Solution {
public:
    void sideview(TreeNode* cur, vector<int>& vec, int depth) {
        if (cur == nullptr) return;
        if (depth == vec.size()) { // 当前深度还没有节点加入结果中，说明当前节点是该深度的第一个节点
            vec.push_back(cur->val); // 将当前节点的值加入结果中
        }
        sideview(cur->right, vec, depth + 1); // 遍历右子树，因为我们希望右子树的节点能够覆盖左子树的节点
        sideview(cur->left, vec, depth + 1); // 遍历左子树
    }

    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        int depth = 0;
        sideview(root, result, depth);
        return result;
    }
};
```

// 当前深度还没有节点加入结果中，说明当前节点是该深度的第一个节点

// 遍历右子树，因为我们希望右子树的节点能够覆盖左子树的节点

//没有右子树，再启用左子树，都不要if和while语句的，因为

```cpp
 if (depth == vec.size()) { // 当前深度还没有节点加入结果中，说明当前节点是该深度的第一个节点
    vec.push_back(cur->val); // 将当前节点的值加入结果中
  }
```



待更新 题目解

## 226反转二叉树（先递归）

做homebrew的Max Howell大佬做不出来的 哈哈

二刷 直接秒杀了

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root==nullptr)return nullptr;
        invertTree(root->left);
        invertTree(root->right);   
        swap(root->left,root->right);
        //左 右 根 所以是后序遍历
        return root;
    }
};
```

后序遍历：先递归反转左子树，再递归反转右子树。再交换根的左右孩子结点。必然是后序。

也可以前序，不建议中序（会导致某些结点左右孩子反转两次）

原因：考虑一个简单的二叉树：

```
    1
   / \
  2   3
```

在中序遍历中，我们首先会遍历到节点2，然后是节点1，最后是节点3。如果我们在遍历到节点1的时候进行左右子树的翻转，那么节点2的左右子树就会被翻转两次：一次是在遍历到节点2的时候，另一次是在遍历到节点1的时候。

## 101对称二叉树（先递归）

```cpp
class Solution {
public:
    bool compare(TreeNode* anode, TreeNode* bnode) {
        if (anode == NULL && bnode != NULL) return false;
        else if (anode != NULL && bnode == NULL) return false;
        else if (anode == NULL && bnode == NULL) return true;
        else if (anode->val != bnode->val) return false;
        bool outside = compare(anode->left, bnode->right);   // 左子树：左、 右子树：右
        bool inside = compare(anode->right, bnode->left);    // 左子树：右、 右子树：左
        bool isSame = outside && inside;                    // 左子树：中、 右子树：中 （逻辑处理）
        return isSame;

    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        return compare(root->left, root->right);
    }
};

```

主要就是说 有几个条件，

左节点空，右节点不空；

左节点不空，右节点空；

左右节点值不一样。

这三种直接返回false就行，但是如何递归的执行下去？

```cpp
bool outside = compare(anode->left, bnode->right); 
        bool inside = compare(anode->right, bnode->left); 
        bool isSame = outside && inside;     
	return isSame；
```

