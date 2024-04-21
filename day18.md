# day18

> [!NOTE]
>
> `!cur->left` 的结果就是判断 `cur->left` 是否为空。



## 513找树左下角的值

本地递归偏难，反而迭代简单属于模板题， 两种方法掌握一下 

一定要是最底层的才行，而不是整个树的左边。

也应该是后序吧，可以直接到左右节点，

其实就是深度最大的叶子节点一定是最后一行。

终止条件：

```cpp
  if (root->left == NULL && root->right == NULL) {
            if (depth > maxDepth) {
                maxDepth = depth;
                result = root->val;
            }
            return;
```

返回值：

```
return result;
```

本层循环：

```cpp
if (root->left) {   // 左
    depth++; // 深度加一
    traversal(root->left, depth);
    depth--; // 回溯，深度减一
}
if (root->right) { // 右
    depth++; // 深度加一
    traversal(root->right, depth);
    depth--; // 回溯，深度减一
}
return;
```

整体代码：

```cpp
class Solution {
private:
    void findfunc(TreeNode*cur,int depth){
        //int maxdepth=-1;
        if(cur->left!=nullptr){
            depth++;
            findfunc(cur->left,depth);
            depth--;
        }
        if(cur->right!=nullptr){
            depth++;
            findfunc(cur->right,depth);
            depth--;
        }

        if(cur->left==nullptr&&cur->right==nullptr){
            if(depth>maxdepth){
                maxdepth=depth;
                result=cur->val;
            }
            return ;
        }
    }
public:
    int depth=0,result=0,maxdepth=-1;
    int findBottomLeftValue(TreeNode* root) {
    findfunc(root,depth);
    return result;
    }
};
```

但是本题就要用层序遍历，比递归好理解。

只需要记录最后一行第一个节点的数值

```cpp
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que; // 创建队列 层序遍历
        if (root != NULL) que.push(root); // 如果根节点不为空，则将根节点加入队列
        int result = 0; // 用于记录最后一行第一个元素的值
        while (!que.empty()) { // 当队列不为空时循环
            int size = que.size(); // 获取当前队列的大小，即当前层的节点个数
            for (int i = 0; i < size; i++) { // 遍历当前层的所有节点
                TreeNode* node = que.front(); // 获取队列的头部节点
                que.pop(); // 将队列的头部节点弹出
                if (i == 0) result = node->val; // 如果是当前层的第一个节点，则更新result的值为当前节点的值
                if (node->left) que.push(node->left); // 如果当前节点有左子节点，则将左子节点加入队列
                if (node->right) que.push(node->right); // 如果当前节点有右子节点，则将右子节点加入队列
            }
        }
        return result; // 返回最后一行第一个元素的值
    }
};
```

1. 首先，在函数开始时，我们创建一个队列 `que` 用于层序遍历，并且如果根节点 `root` 不为空的话，我们将根节点加入队列中。

2. 接下来，我们初始化变量 `result` 为0，用于记录最后一行第一个元素的值。

3. 然后，我们进入一个循环，只要队列不为空就一直循环执行以下步骤：

   a. 获取当前队列的大小 `size`，这个大小表示当前层的节点个数。

   b. 接着，我们进行一个循环，遍历当前层的所有节点。对于每个节点，我们执行以下操作：

   - 从队列中取出一个节点 `node`，这个节点是当前层的一个节点。
   - 如果当前节点是当前层的第一个节点（即 `i` 等于0），我们更新 `result` 的值为当前节点的值。
   - 如果当前节点有左子节点，我们将左子节点加入队列。
   - 如果当前节点有右子节点，我们将右子节点加入队列。

   c. 当内部循环结束后，表示当前层的所有节点都已经处理完毕，我们继续外部的循环。

4. 最终，当外部循环结束后，表示所有层的节点都已经处理完毕，我们返回 `result` 的值，即为最后一行第一个元素的值。

## 112路径总和

本题 又一次设计要回溯的过程，而且回溯的过程隐藏的还挺深，建议先看视频来理解 

路径总和，和 113. 路径总和ii 一起做了。 优先掌握递归法。

> [!IMPORTANT]
>
> 思路：主函数要：
>
> ```
> return traversal(root, sum - root->val);
> ```
>
> 递归函数中：
>
> 后序遍历：
>
> ```cpp
> if (cur->left) { // 左
>             count -= cur->left->val; // 递归，处理节点;
>             if (traversal(cur->left, count)) return true;//如果递归调用返回 true，则表示找到了符合条件的路径，直接返回 true。
>             count += cur->left->val; // 如果递归调用返回 false，则将 count 恢复原值，进行回溯操作。
>         }
> ```

代码：

```cpp
class Solution {
private:
    bool traversal(TreeNode* cur, int count) {
        if (cur->left!=nullptr) { // 左
            count -= cur->left->val; // 递归，处理节点;
            if (traversal(cur->left, count)) return true;//如果递归调用返回 true，则表示找到了符合条件的路径，直接返回 true。
            count += cur->left->val; // 如果递归调用返回 false，则将 count 恢复原值，进行回溯操作。
        }
        if (cur->right!=nullptr) { // 右
            count -= cur->right->val; // 递归，处理节点;
            if (traversal(cur->right, count)) return true;//如果递归调用返回 true，则表示找到了符合条件的路径，直接返回 true。
            count += cur->right->val; // 如果递归调用返回 false，则将 count 恢复原值，进行回溯操作。
        }  
        if (cur->left==NULL && cur->right==NULL && count == 0) return true; // 遇到叶子节点，并且计数为0
        return false;
    }
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) return false;
        return traversal(root, sum - root->val);
    }
};
```

如果说要返回等于targetsum路径所有的节点：

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void traversal(TreeNode* cur, int count) {
        if (cur->left) { // 左 （空节点不遍历）
            path.push_back(cur->left->val);
            count -= cur->left->val;
            traversal(cur->left, count);    // 递归
            count += cur->left->val;        // 回溯
            path.pop_back();                // 回溯
        }
        if (cur->right) { // 右 （空节点不遍历）
            path.push_back(cur->right->val);
            count -= cur->right->val;
            traversal(cur->right, count);   // 递归
            count += cur->right->val;       // 回溯
            path.pop_back();                // 回溯
        }
        if (!cur->left && !cur->right && count == 0) { // 遇到了叶子节点且找到了和为sum的路径
            result.push_back(path);
            return;
        }
        return ;
    }

public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if (root == NULL) return result;
        path.push_back(root->val); // 把根节点放进路径
        traversal(root, sum - root->val);
        return result;
    }
};
```

> [!NOTE]
>
> 总结：
>
> 在主函数中先判root空 再根节点加入path中（如果不需要返回路径，则不需要path数组。还是一样-递归函数的sum要换成sum-root->val;递归函数中可以写前序和后序，1.path加入2.count-=3.递归4.count+=5.path pop

## 106从中序与后序遍历序列构造二叉树

二刷 有点难

```cpp
class Solution {
private:
    TreeNode* traversal (vector<int>& inorder, vector<int>& postorder) {
        if (postorder.size() == 0) return NULL;

        // 后序遍历数组最后一个元素，就是当前的中间节点
        int rootValue = postorder[postorder.size() - 1];
        TreeNode* root = new TreeNode(rootValue);

        // 叶子节点
        if (postorder.size() == 1) return root;

        // 找到中序遍历的切割点
        int delimiterIndex;
        for (delimiterIndex = 0; delimiterIndex < inorder.size(); delimiterIndex++) {
            if (inorder[delimiterIndex] == rootValue) break;
        }

        // 切割中序数组
        // 左闭右开区间：[0, delimiterIndex)
        vector<int> leftInorder(inorder.begin(), inorder.begin() + delimiterIndex);
        // [delimiterIndex + 1, end)
        vector<int> rightInorder(inorder.begin() + delimiterIndex + 1, inorder.end() );

        // postorder 舍弃末尾元素
        postorder.resize(postorder.size() - 1);

        // 切割后序数组
        // 依然左闭右开，注意这里使用了左中序数组大小作为切割点
        // [0, leftInorder.size)
        vector<int> leftPostorder(postorder.begin(), postorder.begin() + leftInorder.size());
        // [leftInorder.size(), end)
        vector<int> rightPostorder(postorder.begin() + leftInorder.size(), postorder.end());

        root->left = traversal(leftInorder, leftPostorder);
        root->right = traversal(rightInorder, rightPostorder);

        return root;
    }
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() == 0 || postorder.size() == 0) return NULL;
        return traversal(inorder, postorder);
    }
};

```

