# day14

## 二叉树基础

### 二叉树的种类

#### 满二叉树

只有度为0和2的结点，并且度为0的结点在同一层

（深度为k 有2^k-1个结点）

#### 完全二叉树

除了最底层可能每天，其余都填满了，

并且最底层的结点集中在该层的左边位置。

![image-20240416110140250](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240416110140250.png)

#### 二叉搜索树

这是有序树，有数值的。左<根<右（中序）

#### 平衡二叉搜索树

大名鼎鼎的avl树

它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

首先是（二叉搜索树的左根右）

其次是（高度差不超过1的平衡树）

**<u>map set 都是平衡二叉搜索树 增删是logn</u>**

**<u>只要加上onordered底层则是哈希表。</u>**

### 二叉树的存储方式

可顺序：数组

可链式：指针

### 二叉树的遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。

2. 广度优先遍历：一层一层的去遍历。

   深度优先遍历

   - 前序遍历（递归法，迭代法）
   - 中序遍历（递归法，迭代法）
   - 后序遍历（递归法，迭代法）

   广度优先遍历

   - 层次遍历（迭代法）

![20200806191109896](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/20200806191109896.png)

### 二叉树的定义

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

## 二叉树的递归

<u>递归的三要素：终 本 返</u>

<u>（终止条件 本层循环 返回值）</u>

二叉树的前序遍历（递归写法）

```cpp
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);    // 中
        traversal(cur->left, vec);  // 左
        traversal(cur->right, vec); // 右
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};

```

*一定要vector<int>&vec*

*原因：需要操作原始result，使用引用传递。而非值传递。*

中序和后序只需要：

改变 如下三行代码的位置。

```cpp
vec.push_back(cur->val);    // 中
traversal(cur->left, vec);  // 左
traversal(cur->right, vec); // 右
```

完成三道题：前序 后序 中序 注意终止条件！

对应：leetcode 144，145，94

## 二叉树的迭代

## 统一迭代

先放过了，考递归解决问题。