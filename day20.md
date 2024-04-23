# day20

> [!NOTE]
>
> return用作：return递归的上一层，而不一定一定是最后结果。

###  **654.最大二叉树** 

又是构造二叉树，昨天大家刚刚做完 中序后序确定二叉树，今天做这个 应该会容易一些， 先看视频，好好体会一下 为什么构造二叉树都是 前序遍历 

这个留着等二刷，昨天的没搞（因为）

### **617.合并二叉树** 

> [!NOTE]
>
> ```cpp
>   if(root2==nullptr){
>             return root1;
>         }//merge的思路
> 
>  root1->val+=root2->val;//合并值的思路
> //或者定义新树
>  TreeNode* root = new TreeNode(0);
> ```

这次是一起操作两个二叉树了， 估计大家也没一起操作过两个二叉树，也不知道该如何一起操作，可以看视频先理解一下。 优先掌握递归。

两个二叉树的合并；

如果相同的节点，就加起来。不相同就合并为一个树。

（考察一起操作两个二叉树）

中左右：前序遍历 比较符合逻辑。

1.终止条件（两个树是同步遍历的 可以互相return return是return给递归的上一层）

```cpp
        if(root1==nullptr){
            return root2;//因为两个树是同步遍历的 可以直接return
        }
        if(root2==nullptr){
            return root1;
        }
```

2.返回值

```cpp
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2)
```

3.本层循环（直接在一棵树上操作

```cpp
 root1->left=mergeTrees(root1->left,root2->left);
        root1->right=mergeTrees(root1->right,root2->right);//
        root1->val+=root2->val;//可以后序 也可以前序 也可以中序
```

4.整体代码

```cpp
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1==nullptr){
            return root2;//因为两个树是同步遍历的 可以直接return
        }
        if(root2==nullptr){
            return root1;
        }
        root1->left=mergeTrees(root1->left,root2->left);
        root1->right=mergeTrees(root1->right,root2->right);//
        root1->val+=root2->val;//可以后序 也可以前序 也可以中序
        //最后合成为一个树 就是root1的树
        return root1;
    }
};
```

### **700.二叉搜索树中的搜索** 

递归和迭代 都可以掌握以下，因为本题比较简单， 了解一下 二叉搜索树的特性。

> [!TIP]
>
> #### 二叉搜索树bst
>
> 这是有序树，有数值的。左<根<右（中序）
>
> #### 平衡二叉搜索树
>
> 大名鼎鼎的avl树
>
> 它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
>
> 首先是（二叉搜索树的左根右）
>
> 其次是（高度差不超过1的平衡树）
>
> **<u>map set 都是平衡二叉搜索树 增删是logn</u>**
>
> **<u>只要加上onordered底层则是哈希表。</u>**

1.终止条件

```cpp
if (root == NULL || root->val == val) return root;
```

2.返回值

```cpp
TreeNode* searchBST(TreeNode* root, int val)
```

3.本层循环

```

```

4.整体代码

```cpp
//递归
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
    if (root == NULL || root->val == val) return root;
    TreeNode* result = NULL;
        if (root->val > val) result = searchBST(root->left, val);
        if (root->val < val) result = searchBST(root->right, val);
    return result;
    }
};
//迭代
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        TreeNode* result=nullptr;
        while(root!=nullptr){
            if(root->val>val)root=root->left;
            else if(root->val<val)root=root->right;
            else {result=root;break;}
        }
    return result;
    }
};
```

尽量写if语句不要简写{}容易出错

### **98.验证二叉搜索树** 

遇到 搜索树，一定想着中序遍历，这样才能利用上特性。 

但本题是有陷阱的，可以自己先做一做，然后在看题解，看看自己是不是掉陷阱里了。这样理解的更深刻。



**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 小于 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。



终止条件

```cpp
if (root == NULL) return true;
```

返回值

```cpp
long long maxVal = LONG_MIN; // 因为后台测试数据中有int最小值
bool isValidBST(TreeNode* root)

```

整体：

```cpp
class Solution {
public:
long long maxval=LONG_MIN;
    bool isValidBST(TreeNode* root) {
        if(root==nullptr)return true;
        bool left=isValidBST(root->left);
        if(root->val>maxval){maxval=root->val;}
        else return false;
        bool right=isValidBST(root->right);
        return left&&right;
    }
};
```

简单题 要学会思想。

> [!WARNING]
>
> 大体是：左根右
>
> 左就遍历即可 bool返回
>
> 根要判决 如果maxval的大 更新maxval 如果小 就是错
>
> 右就遍历即可 bool返回
>
> 返回的是左和右都满足 就可以返回true

或者

中间改为：

```cpp
 if (pre != NULL && pre->val >= root->val) return false;
        pre = root; // 记录前一个节点
```

#### 递归解决的全部流程：中序遍历会直接到最左边的子节点，我就然后再它的父节点，再看看父节点有没有右节点，没有就再左中就可以。假如有右了，也是先到右子树的左下角，左中，再左中右。

