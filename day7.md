# day7

## 454四数相加（哈希经典）

<u>建议：</u>本题是 使用map巧妙解决的问题，好好体会一下 哈希法如何提高程序执行效率，降低时间复杂度，当然使用哈希法 会提高空间复杂度，但一般来说我们都是舍空间换时间， 工业开发也是这样。

<u>题目分析：</u>四个数组，计算有多少个排列组合，能够使得每一个数组取一个数字，相加起来为0

<u>思路：</u>判断一个元素是否出现在集合中，就要考虑哈希。

<u>注意点：</u>和四数之和有区别的，但是简单一点

<u>暴力：</u>四个for循环，on^4

由于：-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228

<u>结论：</u>所以使用的是set或者map来统计a+b，以及次数。

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
    unordered_map<int,int>map;
    int count=0;
    for(int a:nums1){
        for(int b:nums2){
            map[a+b]++;
        }
    }//遍历a和b 总结到一个map里 key是a+b value是次数
    for(int c:nums3){
        for(int d:nums4){
            auto it=map.find(0-(c+d));
            if(it!=map.end()){
            count+=map[0-(c+d)];
            }
        }
    }
    return count;
    }
};
```

<u>总结：</u>四数相加，其实也可以类比成为两数之和，也就是leetcode1，思路一样，所以可以把前两个数组循环调用，得到map[a+b]++;一个key为a+b，value是出现次数的哈希map，再循环剩下两个数组，有一个等式：0-（c+d）=a+b

这样的情况下，实际上就是我们所求的目标，所以在第二个双重循环下，我们直接找到有没有这样的0-（c+d）即可。

<u>和leetcode1不同点：</u>本题需要知道的是a+b和a+b的次数，而leetcode1需要知道的是a以及它的下标存进value中。

<u>注意：</u>和leetcode统一使用，迭代器来看，迭代器一般用auto it=map.find(0-(c+d)),也就是map调用find会返回一个迭代器，迭代器可以判断是不是哈希表的结尾。也可以方便返回map的key和value。

## 383赎金信

<u>题目：</u>ransomnote是magazine的子集的意思，小写英文

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int>map;
        for(auto a:magazine){
            map[a]++;
        }
        for(auto a:ransomNote){
            map[a]--;
        }
        for(auto it=map.begin();it!=map.end();++it){
            if(it->second<0){
                return false;
            }
        }


        return true;
    }
};
```

<u>总结：</u>同242字母异位词

## 15三数之和

两数之和可以存进map里面

但是这个涉及到去重问题，但也可以做啊：两个循环+一个

但是abc都要去重，得多想想。

思路：i+双指针，i和left还有right都要去重，但是去重的方法不一样

i的去重方法：

```cpp
 if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
```

left和right的去重方法：

```cpp
 result.push_back({nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    right--;
```

所以还需注意：这题返回的是个二维数组

二维数组的定义是：

```cpp
vector<vector<int>>result;
```

二维数组插入一行元素的代码是：

```cpp
result.push_back({nums[i],nums[left],nums[right]})
```

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                return result;
            }
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                if (nums[i] + nums[left] + nums[right] > 0) right--;
                else if (nums[i] + nums[left] + nums[right] < 0) left++;
                else {
                    result.push_back({nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    right--;
                    left++;
                }
            }
        }
        return result;
    }
};

```

## 18四数之和

剪枝：

```cpp
            if (nums[k] > target && nums[k] >= 0) {
            	break; // 这里使用break，统一通过最后的return返回
            }
```

注意：剪枝操作必须是nums【k】>0，因为如果target是负数，那么即使比它大，也有可能加上后面的整数而满足条件。（也可以选择不剪枝，即我不管他第一个i和target的关系了）

去重：

```cpp
if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
```

记住：如果很多个数相加，可以相加式子前面直接弄个（long）

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            // 剪枝处理
         //   if (nums[k] > target && nums[k] >= 0) {
          //  	break; // 这里使用break，统一通过最后的return返回
         //   }
            // 对nums[k]去重
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                // 2级剪枝处理
         //       if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0) {
           //         break;
           //     }

                // 对nums[i]去重
                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if ((long) nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    // nums[k] + nums[i] + nums[left] + nums[right] < target 会溢出
                    } else if ((long) nums[k] + nums[i] + nums[left] + nums[right]  < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 对nums[left]和nums[right]去重
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        // 找到答案时，双指针同时收缩
                        right--;
                        left++;
                    }
                }

            }
        }
        return result;
    }
};

```

