# day24

## 回溯算法理论

回溯-回溯搜索法：就是搜索的方式

有递归就有回溯。（所以回溯函数就是递归函数）

所有回溯法的问题都可以抽象为树形结构！

回溯法三部曲：

返回值：一般为void

```cpp
void backtracking(参数)
```

终止条件：

```cpp
if (终止条件) {
    存放结果;
    return;
}
```

遍历过程：**for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历**，这样就把这棵树全遍历完了，一般来说，搜索叶子节点就是找的其中一个结果了。

```cpp
for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
    处理节点;
    backtracking(路径，选择列表); // 递归
    回溯，撤销处理结果
}
```

## 77组合（排列组合的组合 不重复）

题目：给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

想法：两层for循环？一个是1一个是i+1 但是不止两层for循环，取决于k值。

但是回溯也是暴力，它怎么嵌套for循环？用递归。

集合是：1，2，3，4

取1可以配234，再从234取2就成了1-2。（树形结构）

```cpp
class Solution {
private:
    vector<vector<int>>result;
    vector<int>path;
    void backtracking(int n,int k,int startindex){
    for(int i=startindex;i<=n;i++){
        path.push_back(i);//取了个1
        backtracking(n,k,i+1);
        path.pop_back();//取到12之后 要弹出2  才能加3   弹出3  才能加入4
    } 
    if(path.size()==k){//if可以随时判断
        result.push_back(path);
    }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);//这个1就是范围 [1, n]的左边界
        return result;
    }
};
```

总之想象成一棵树 递归为了更深 但是最深就是path=k

for为了更宽 最宽可以是n - (k - path.size()) + 1（剪枝了）

**如果for循环选择的起始位置之后的元素个数 已经不足 我们需要的元素个数了，那么就没有必要搜索了**。