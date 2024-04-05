# day1

## 数组基础

存放在 连续空间 相同类型

下标从0开始

vector是容器而不是数组

## 704二分查找

**题目建议**： 大家能把 704 掌握就可以，35.搜索插入位置 和 34. 在排序数组中查找元素的第一个和最后一个位置 ，如果有时间就去看一下，没时间可以先不看，二刷的时候在看。

题目：升序 nums target 有就返回下标

必然是二分ologn o1

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0,r=nums.size()-1;
        while(l<=r){
            int mid=l+((r-l)/2);
            if(target<nums[mid]){
                r=mid-1;
            }
            else if(target>nums[mid]){
                l=mid+1;
            }
            else{
                return mid;
            }
        }
        return -1;
    }
};
```

### 35搜索插入位置

排好序 目标值 返回被插入的位置

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l=0,r=nums.size()-1;
        
        while(l<=r){int mid=l+((r-l)/2);
            if(target<nums[mid]){
                r=mid-1;
            }
            else if(target>nums[mid]){
                l=mid+1;
            }
            else{
                return mid;
            }
        }
        return r+1;
    }
};
```

注意！return r+1思想 以及mid必须放在while里 不然更新不了

## 27移除元素

暴力+双指针 双掌握

暴力：

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (nums[i] == val) { // 发现需要移除的元素，就将数组集体向前移动一位
                for (int j = i + 1; j < size; j++) {
                    nums[j - 1] = nums[j];
                }
                i--; // 因为下标i以后的数值都向前移动了一位，所以i也向前移动一位
                size--; // 此时数组的大小-1
            }
        }
        return size;

    }
};
```

![image-20240403145602117](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240403145602117.png)

快慢指针：

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow=0;
        int fast=0;
        for(fast;fast<nums.size();fast++){
            if(nums[fast]!=val)
            {
                nums[slow]=nums[fast];
                slow++;
            }
        }
        return slow;
    }
};
```

![image-20240403145757258](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240403145757258.png)

