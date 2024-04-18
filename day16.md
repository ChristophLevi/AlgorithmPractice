# day16

## 104二叉树的最大深度（优先递归）

深度：从根结点到最远叶子结点的最长路径的节点数。

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
       if(root==nullptr)return 0;
       int leftdep=maxDepth(root->left);//就是根节点的左子树 左子树上不管是左节点还是右节点 都靠这句话
       int rightdep=maxDepth(root->right);//同上 类比
        return max(leftdep,rightdep)+1;
    }
};
```

n叉树：

`root->children` 表示 N 叉树节点的子节点列表。在 N 叉树中，每个节点可以有多个子节点，因此 `root->children` 是一个存储子节点的列表或数组。

为什么n叉树要用循环 递归？

因为子节点数量不固定！

```cpp
class Solution {
public:
    // 定义一个函数用于计算 N 叉树的最大深度
    int maxDepth(Node* root) {
        // 如果节点为空，返回深度 0
        if (root == nullptr) return 0;
        int depth = 0; // 初始化深度为 0
        // 遍历节点的子节点
        for (int i = 0; i < root->children.size(); i++) {
            // 递归调用 maxDepth 函数计算每个子节点的最大深度
            depth = max(depth, maxDepth(root->children[i]));
        }
        // 返回子节点中最大深度加 1，即为整棵树的最大深度
        return depth + 1;
    }
};
```

## 111二叉树的最小深度（优先递归）

和最大深度 看似差不多，其实 差距还挺大，有坑。

递归三部曲：

1.终止条件：

```cpp
if (node == NULL) return 0;
```

2.本层循环

```cpp
int leftDepth = getDepth(node->left);           // 左
int rightDepth = getDepth(node->right);         // 右
                                                // 中
// 当一个左子树为空，右不为空，这时并不是最低点
if (node->left == NULL && node->right != NULL) { 
    return 1 + rightDepth;
}   
// 当一个右子树为空，左不为空，这时并不是最低点
if (node->left != NULL && node->right == NULL) { 
    return 1 + leftDepth;
}
int result = 1 + min(leftDepth, rightDepth);
return result;
```

3.返回值

```cpp
int getDepth(TreeNode* node)
```

所以返回int



所以，如果左子树为空，右子树不为空，说明最小深度是 1 + 右子树的深度。（一定要有递归的思想：return 1 + rightDepth;）

反之，右子树为空，左子树不为空，最小深度是 1 + 左子树的深度。 （一定要有递归的思想：return 1 + leftDepth;）

## 222完全二叉树的节点个数

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
    if(root==nullptr)return 0;
    int leftsum=countNodes(root->left);
    int rightsum=countNodes(root->right);
    int result=leftsum+rightsum+1;
    return result;
    }
};
```

和求深度没区别