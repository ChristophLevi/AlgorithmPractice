# day2

## 977有序数组的平方

非递减 nums 返回每树平方 非递减

想法：左右双指针 判断谁的平方更大 就先更大的平方存入新数组 然后更大的往里面缩一个 持续判断 持续存入新数组

想法是对的 想想实现

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int>newnum(nums.size());
        int left=0,right=nums.size()-1;
        int size=nums.size()-1;
        while(left<=right){
            if(nums[left]*nums[left]<=nums[right]*nums[right]){
                newnum[size]=nums[right]*nums[right];
                right--;
                size--;
            }
            else {
                newnum[size]=nums[left]*nums[left];
                left++;
                size--;
            }
        }
        return newnum;
    }
};
```

要注意：vector<int>newnum(nums.size())这个定义别写错

这样的代码时间是on（左右指针为on）

![image-20240404114951210](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240404114951210.png)

也可以直接暴力啊 直接循环存入+sort快排

onlogn

## 209长度最小的子数组

和大于等于target的连续子数组 物理连续 而不是数字连续

所以就是滑动窗口的感觉 先右边后左边指针滑动起来

不存在即为return 0；

窗口的起始位置滑动靠“窗口内总和大于了target”

窗口的结束位置滑动靠“循环的遍历”

而且找最小：要想到先定义int result=INT32_MAX;

然后靠判断表达式来不断迭代result

出现的问题：result判别应该在sum更新和i自增的前面

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int i=0,j=0,sum=0;
        int window=0;
        int result=INT32_MAX;
        for(j=0;j<=nums.size()-1;j++){
            sum+=nums[j];
            while(sum>=target){
                window=j-i+1;result=result<window?result:window;
                sum=sum-nums[i];i++;
                
            }
       
        }    
         if(result!=INT32_MAX){
                return result;
            }else{
                return 0;
            }
    }
};
//时间复杂度 o2n   还是on 因为每个元素被操作的次数是进去一次 出去一次
```

![image-20240404214148690](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240404214148690.png)

## 59螺旋矩阵II

不想看题解

有个简单的做法

上边界top 下边界bot 左边界left 右边界right

![image-20240404222403097](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240404222403097.png)

上右下左这样子遍历

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
    int left=0;
    int right=n-1;
    int top=0;
    int bot=n-1;
    int max=n*n;
    int num=1;
    vector<vector<int>>result(n,vector<int>(n));
    while(num<=max){
    for(int temp=left;temp<=right;temp++){
        result[top][temp]=num;
        num++;
    }
    top++;
     for(int temp=top;temp<=bot;temp++){
        result[temp][right]=num;
        num++;
    }
    right--;
     for(int temp=right;temp>=left;temp--){
        result[bot][temp]=num;
        num++;
    }
    bot--;
     for(int temp=bot;temp>=top;temp--){
        result[temp][left]=num;
        num++;
    }
    left++;
    }
    return result;
    }
};

```

太形象了 卡边界 做本填入 判出条件为while（num<=n*n）

## 数组总结

二分-------------------ologn 注意闭区间

插入元素------------暴力+快慢双指针 双指针就是on

有序数组平方： 左右双指针on

长度最小：    滑动窗口 on

螺旋矩阵 直接模拟法 上下左右卡住