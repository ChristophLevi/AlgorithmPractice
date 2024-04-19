# day17

## 110平衡二叉树（优先递归）

判断是不是所有节点的左右子树的深度相差不超过 1。

其实是判断高度。

所以采用：后序遍历+递归

递归三部曲：

1.终止条件

```cpp
if(root==nullptr)return true;
```

2.返回值和参数

```cpp
    bool isBalanced(TreeNode* root)
```

返回的是return true

3.本层循环的工作

后序 求高度

```cpp
class Solution {
public:
    // 返回以该节点为根节点的二叉树的高度，如果不是平衡二叉树了则返回-1
    int getHeight(TreeNode* node) {
        if (node == NULL) {
            return 0;
        }
        int leftHeight = getHeight(node->left);
        if (leftHeight == -1) return -1;
        int rightHeight = getHeight(node->right);
        if (rightHeight == -1) return -1;
        return abs(leftHeight - rightHeight) > 1 ? -1 : 1 + max(leftHeight, rightHeight);
    }
    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;
    }
};
```

如果左右子树的高度差不超过1，我们返回以该节点为根的子树的实际高度，即1加上左右子树中较大的高度。如果左右子树的高度差超过1，我们返回-1表示不是平衡二叉树。

## 257二叉树的所有路径（优先递归）

这是大家第一次接触到回溯的过程， 我在视频里重点讲解了 本题为什么要有回溯，已经回溯的过程。 

如果对回溯 似懂非懂，没关系， 可以先有个印象。 

```cpp
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

递归：前中后序 路径一定要用**前序遍历** 这样才能父->孩子

收集路径的思想，如何让125之后再把5和2弹出去，再加入3呢？

这就是**回溯**的过程。

终止条件：

```cpp
 if(node->left==nullptr&&node->right==nullptr){
        result.push_back(path);//注意path要转成string 而且要有箭头加入
    }
```

因为本题要找到叶子节点，就开始结束的处理逻辑了（把路径放进result里）。

**那么什么时候算是找到了叶子节点？** 是当 cur不为空，其左右孩子都为空的时候，就找到叶子节点。

本层循环：

```cpp
vector<string> ans; // 保存最终结果的数组
    string path; // 保存当前路径的字符串
    
    void dfs(TreeNode* now) // 深度优先搜索函数
    {
        if(now==NULL) // 如果当前节点为空，直接返回
           return;
        string tmp=path; // 保存当前路径
        path+=(path==""?"":"->")+to_string(now->val); // 更新路径
        if(now->left==NULL && now->right==NULL) // 到达一个叶子结点，保存该路径
        ans.push_back(path); // 将路径添加到结果数组中
        dfs(now->left); // 递归遍历左子树
        dfs(now->right); // 递归遍历右子树
        path=tmp; // 回溯，恢复当前路径
    }
```

返回值：

```cpp
void traversal(TreeNode* cur, vector<int>& path, vector<string>& result)
```

整体代码：

```cpp
class Solution {
public:
    vector<string> ans; // 保存最终结果的数组
    string path; // 保存当前路径的字符串
    
    void dfs(TreeNode* now) // 深度优先搜索函数
    {
        if(now==NULL) // 如果当前节点为空，直接返回
           return;
        string tmp=path; // 保存当前路径
        path+=(path==""?"":"->")+to_string(now->val); // 更新路径
        if(now->left==NULL && now->right==NULL) // 到达一个叶子结点，保存该路径
        ans.push_back(path); // 将路径添加到结果数组中
        dfs(now->left); // 递归遍历左子树
        dfs(now->right); // 递归遍历右子树
        path=tmp; // 回溯，恢复当前路径
    }
    
    vector<string> binaryTreePaths(TreeNode* root) { // 主函数
        dfs(root); // 调用深度优先搜索函数
        return ans; // 返回结果数组
    }
};
```

*如果 `path` 为空，那么括号内的表达式的值就是 `to_string(now->val)`，所以相当于 `path = path + to_string(now->val)`；如果 `path` 不为空，那么括号内的表达式的值就是 `"->" + to_string(now->val)`，所以相当于 `path = path + "->" + to_string(now->val)`。*

carl版：

```cpp
class Solution {
private:
    void traversal(TreeNode* cur, string path, vector<string>& result) {
        path += to_string(cur->val); // 中，中为什么写在这里，因为最后一个节点也要加入到path中
        if (cur->left == NULL && cur->right == NULL) {
            result.push_back(path);
            return;
        }
        if (cur->left) {
            path += "->";
            traversal(cur->left, path, result); // 左
            path.pop_back(); // 回溯 '>'
            path.pop_back(); // 回溯 '-'
        }
        if (cur->right) {
            path += "->";
            traversal(cur->right, path, result); // 右
            path.pop_back(); // 回溯'>'
            path.pop_back(); // 回溯 '-'
        }
    }

public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        string path;
        if (root == NULL) return result;
        traversal(root, path, result);
        return result;

    }
};
```

## 404左叶子之和（优先递归）

终止条件：

```cpp
if(cur==nullptr){
        return 0;
    }
```

返回值：

```cpp
void leftleaves(TreeNode*cur,int &sum)
    //返回的是sum
```

本层循环：

```cpp
int leftval=leftleaves(cur->left,sum);
    if (cur->left != NULL && cur->left->left == NULL && cur->left->right == NULL) {
    leftval = cur->left->val;
    } 
    int rightval=leftleaves(cur->right,sum);
    sum=leftval+rightval;
```

整体代码：

```cpp
class Solution {
private:
    int leftleaves(TreeNode*cur,int &sum){
    if(cur==nullptr){
        return 0;
    }
    int leftval=leftleaves(cur->left,sum);
    if (cur->left != NULL && cur->left->left == NULL && cur->left->right == NULL) {
    leftval = cur->left->val;
    } 
    int rightval=leftleaves(cur->right,sum);
    sum=leftval+rightval;
    return sum;
    }
public:
    int sumOfLeftLeaves(TreeNode* root) {
    int sum=0;
    leftleaves(root,sum);
    return sum;
    }
};
```

注意：后序遍历，要取的是满足：

(cur->left != NULL && cur->left->left == NULL && cur->left->right == NULL)条件的值

递归知识为了遍历整个树，所以左右子树都要取出来这个满足的

满足的就让leftval=cur->left->val;

最重要的就是：取左叶子，必须直到根节点cur再弄。