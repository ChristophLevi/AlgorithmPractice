# day32

## 122买卖股票的最佳时机ii

https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

输入: [7,1,5,3,6,4]

输出: 7 

就是第二天买了，第三天卖了。第四天买了，第五天卖了。4+3=7

**局部最优：收集每天的正利润，全局最优：求得最大利润**。贪心的思想就是：贪好每一次，不管最后是不是最佳。

就是来一个每天利润数组，统计每天和昨天的差值。只要是正的，我们就可以买了，计入。tip：第一天无利润。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        for (int i = 1; i < prices.size(); i++) {
            result += max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
};
```

## 55跳跃游戏

本题如果没接触过，很难想到，所以不要自己憋时间太久，读题思考一会，没思路立刻看题解 

想法：

**每次取最大跳跃步数（取最大覆盖范围），整体最优解：最后得到整体最大覆盖范围，看是否能到终点**。

虽然是最大跳跃步数，但是输入: [3,2,1,0,4]输出: false这个例子中如果0换成1就可以了，但是1<2并不最大。

所以还是一种滑动窗口的思想，让cover覆盖好离最后一位的最大范围，检测最后一个在不在范围就可以了。

怎么动态更新cover？ cover=max(i+nums[i],cover)

循环体的终结点 <=cover即可 有点剪枝的感觉 其实也可以<nums.size()(可能)

![image-20240511220323948](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240511220323948.png)

```
class Solution {
public:
    bool canJump(vector<int>& nums) {
    int cover=0;
    if(nums.size()==1)return true;
    for(int i=0;i<=cover;i++){
    cover=max(i+nums[i],cover);
    if(cover>=nums.size()-1)return true;
    }
    return false;
    }
};
```

## 45跳跃游戏ii

本题同样不容易想出来。贪心就是这样，有的时候 会感觉简单到离谱，有时候，难的不行，主要是不容易想到。

[1,2,1,1,1]预期结果是3 但是如果按照上面的思路来，那将会是4 因为它要遍历的，怎么跳过中间的1呢？

```
// 版本一
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int curDistance = 0;    // 当前覆盖最远距离下标
        int ans = 0;            // 记录走的最大步数
        int nextDistance = 0;   // 下一步覆盖最远距离下标
        for (int i = 0; i < nums.size(); i++) {
            nextDistance = max(nums[i] + i, nextDistance);  // 更新下一步覆盖最远距离下标
            if (i == curDistance) {                         // 遇到当前覆盖最远距离下标
                ans++;                                  // 需要走下一步
                curDistance = nextDistance;             // 更新当前覆盖最远距离下标（相当于加油了）
                if (nextDistance >= nums.size() - 1) break;  // 当前覆盖最远距到达集合终点，不用做ans++操作了，直接结束
            }
        }
        return ans;
    }
};
 
```

