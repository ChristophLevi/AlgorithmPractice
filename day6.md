# day6

day5系周日休息

## 哈希表基础

1.建议：要了解哈希表的内部实现原理，哈希函数，哈希碰撞，以及常见哈希表的区别，数组，set 和map。 

2.什么时候想到用哈希法，**当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**。 这句话很重要，大家在做哈希表题目都要思考这句话。 

3.一般哈希表都是用于：快速判断一个元素是否出现在集合里。

4.o1查找该元素的方式

5.哈希函数（取模），哈希碰撞。

6.哈希碰撞解决方法：

拉链法：碰撞之后类似拉链一样，选择适当的哈希表大小

线性探测法：哈希表要大，放不下就下一个空位

7.

![image-20240408183949629](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240408183949629.png)

![image-20240408184009389](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240408184009389.png)

set三兄弟：set multiset unordered_set 红红哈

最重要的是：红黑树底层会导致key值有序，不可修改

map三兄弟：map multimap unordered_map

最重要的是：和set的情况一样，类比

总结：需要有序，就用前两个。解决哈希肯定用unordered

哈希就是c++11标准库的官方轮子，牺牲空间换时间。

## 242有效的字母异位词

题目：给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `*t*` 是否是 `*s*` 的字母异位词。

**注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `*s*` 和 `*t*` 互为字母异位词。

思路：想到的就是哈希表，如何对比两个字符串的哈希是否相同？

答案：   

```cpp
for (auto it = charCount.begin(); it != charCount.end(); ++it) {
    if (it->second != 0) {
        return false;
    }
}
```

哈希表法-整体代码：

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }
        unordered_map<char, int> charCount;
        for (char ch : s) {
            charCount[ch]++;
        }
        for (char ch : t) {
            charCount[ch]--;
        }

for (auto it = charCount.begin(); it != charCount.end(); ++it) {
    if (it->second != 0) {
        return false;
    }
}
        return true;
    }
};
```

总结：一个循环让哈希表自增，另一个循环让哈希表自减，如果出现哈希值小于0，必然是不对的，这个有点巧妙。

注意：不要用string类型，记住不为零就return false

注意记住哈希表的遍历

这样的时空复杂度：均为o（n）

法二-数组法：字符a到字符z的ASCII是26个连续的数值。

这个就是哈希的思想，但是没用哈希表而已。获得o1空间。

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++) {
            record[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (record[i] != 0) {
                // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        // record数组所有元素都为零0，说明字符串s和t是字母异位词
        return true;
    }
};
```

## 349两个数组的交集

建议：本题就开始考虑 什么时候用set 什么时候用数组，本题其实是使用set的好题，但是后来力扣改了题目描述和测试用例，添加了 0 <= nums1[i], nums2[i] <= 1000 条件，所以使用数组也可以了，不过建议大家忽略这个条件。 尝试去使用set。 

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> countMap;
        unordered_set<int> resultSet;
        for (int num : nums1) {
            countMap[num]++;
        }
        for (int num : nums2) {
            if (countMap.find(num) != countMap.end()) {
                resultSet.insert(num);
            }
        }
        vector<int> result(resultSet.begin(), resultSet.end());
        return result;
    }
};
```

重要：遍历nums2，如果元素在countMap中存在，则加入resultSet

```cpp
 if (countMap.find(num) != countMap.end()) {
                resultSet.insert(num);
            }
```

注意转化set为vector

```cpp
vector<int> result(resultSet.begin(), resultSet.end());
```

## 202快乐数

快乐数：

![image-20240408200015048](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240408200015048.png)

想法：必须最后得是1和一些零的情况下，才可能是快乐数。

怎么得到平方和，还能拆分各自一位数呢？

感觉是数学问题，但又：

这道题目使用**哈希法**，来判断这个sum是否重复出现，如果重复了就是return false， 否则一直找到sum为1为止。

判断sum是否重复出现就可以使用unordered_set。

**还有一个难点就是求和的过程，如果对取数值各个位上的单数操作不熟悉的话，做这道题也会比较艰难。**

```cpp
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> set; // 用于存储出现过的平方和
        while(1) {
            int sum = 0;
            while (n) {
                sum += (n % 10) * (n % 10); // 计算各个位上数字的平方和
                n /= 10; // 去掉最低位，继续计算下一位
            }
            if (sum == 1) {
                return true; // 如果平方和等于1，说明是快乐数，返回true
            }
            // 如果这个sum曾经出现过，说明已经陷入了无限循环了，立刻return false
            if (set.find(sum) != set.end()) {
                return false; // 如果平方和曾经出现过，说明陷入了无限循环，返回false
            } else {
                set.insert(sum); // 将当前的平方和加入到集合中
            }
            n = sum; // 更新n为当前的平方和，继续迭代
        }
    }
};
```

先学会各个位数字的平方和（sum是和，n是本数）-->想好迭代怎么做（如果没有重复出现，就存进set里面，本数变成和）-->重复直到sum为1返回true，直到发现sum在set中发现过，就返回false

注：都在一个循环中，直接用while（1）{}

## 1两数之和

map中的存储结构为

 {key：数据元素，value：数组元素对应的下标}

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int> &nums, int target) {
        unordered_map<int, int> idx; // 创建一个空哈希表

        for(int i=0;i<nums.size();i++){
            idx[nums[i]] = i;
        }
        for (int j = 0; j < nums.size(); j++) { // 枚举 j
            auto it = idx.find(target - nums[j]);
            if (it != idx.end() && it->second != j) { // 找到了，并且不是同一个元素
                return {it->second, j}; // 返回两个数的下标
            }
        }
        return {}; // 如果没有找到符合条件的两个元素，返回空数组
    }
};
```

先存一个键值对------在find一下，如果找到了且不是同一个玄素，可以返回