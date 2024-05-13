# day33

##  1005.K次取反后最大化的数组和 

本题简单一些，估计大家不用想着贪心 ，用自己直觉也会有思路。 

```cpp
输入：nums = [4,2,3], k = 1
输出：5
解释：选择下标 1 ，nums 变为 [4,-2,3] 。
```

想法：无序得先排个序，然后直接弄最小的k个，如果最小的k个有正数，那就重复弄

```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        // 先对数组进行排序
        sort(nums.begin(), nums.end());
        // 进行K次取反，每次只需要把第一个元素值取反即可，取反后再排序
        for(int i = 0; i < k; i++){
            // 做剪枝操作，对0多次取反没有意义
            if(nums[0] == 0){
                break;
            }
            nums[0] = -nums[0];
            sort(nums.begin(), nums.end());
        }
        // 求累加和
        int sum = 0;
        for(auto item : nums){
            sum += item;
        }
        return sum;
    }
};
```

上面这个代码太妙了 直接每次就反转第一个 反转完就排序，最后累加和。

## 134加油站 

本题有点难度，不太好想，推荐大家熟悉一下方法二 

暴力：

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        for (int i = 0; i < cost.size(); i++) {
            int rest = gas[i] - cost[i]; // 记录剩余油量
            int index = (i + 1) % cost.size();
            while (rest > 0 && index != i) { // 模拟以i为起点行驶一圈（如果有rest==0，那么答案就不唯一了）
                rest += gas[index] - cost[index];
                index = (index + 1) % cost.size();
            }
            // 如果以i为起点跑一圈，剩余油量>=0，返回该起始位置
            if (rest >= 0 && index == i) return i;
        }
        return -1;
    }
};
```

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {   // 当前累加rest[i]和 curSum一旦小于0
                start = i + 1;  // 起始位置更新为i+1
                curSum = 0;     // curSum从0开始
            }
        }
        if (totalSum < 0) return -1; // （大前提就是：如果每次gas-cost最终就是个负数，那是永远不可能的）
        return start;
    }
};
```

上面的这个贪心，写的非常得妙 因为都是往右边一个加油站跑 所以可以弄两个sum 只要变成负的 就说明从原先的道路不可能能走 所以可以start=i+1 并且还不用动start 让它在那里就好  因为start在循环中 每次都会执行的 也会自己更新的 最后返回start就可以了 如果另一个sum<0说明不可能（这个很关键，这是最终的卡的东西 卡全局sum）（大前提就是：如果每次gas-cost最终就是个负数，那是永远不可能的）

## 135分发糖果 

本题涉及到一个思想，就是想处理好一边再处理另一边，不要两边想着一起兼顾，后面还会有题目用到这个思路 