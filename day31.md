# day31

## 贪心 理论基础

**贪心算法其实就是没有什么规律可言**，所以了解贪心算法 就了解它没有规律的本质就够了。 

学完贪心之后再去看动态规划，就会了解贪心和动规的区别。

**本质是选择每一阶段的局部最优，从而达到全局最优**。

局部先最优--综合起来就可以全局最优（甚至不需要推导为什么选）**常识性推导加上举反例**。

## 455分发饼干

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int index = s.size() - 1; // 饼干数组的下标
        int result = 0;
        for (int i = g.size() - 1; i >= 0; i--) { // 遍历胃口
            if (index >= 0 && s[index] >= g[i]) { // 遍历饼干
                result++;
                index--;
            }
        }
        return result;
    }
};
```

看到要遍历两个数组，就想到用两个 for 循环，那样逻辑其实就复杂了。

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int index = 0;
        for(int i = 0; i < s.size(); i++) { // weikou
            if(index < g.size() && g[index] <= s[i]){ // 胃口
                index++;
            }
        }
        return index;
    }
};
```

> [!NOTE]
>
> 这道题最注意的是：如果想要小饼干满足小胃口，必须让饼干作为for循环，因为才能满足不了就自增饼干。

## 376摆动序列



## 53最大子序和