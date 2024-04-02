# algorithm dayx01

## 数组理论

**C++中二维数组在地址空间上是连续的**。

## 704 二分查找

~有序+无重复可以直接：二分法

~定义在双闭区间

~while (left <= right) 要使用 <= ，因为left == right是有意义的

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left=0;
        int right=nums.size()-1;
        while(left<=right){
        int mid=left+(right-left)/2;  
        if(nums[mid]<target){
            left=mid+1;
        }  
        else if(nums[mid]>target){
            right=mid-1;
        }else if(nums[mid]==target)
        {
            return mid;
        }
        }
        return -1;
    }
};//自己ac的
```

时间ologn 空间o1

tips：**区间的定义就是不变量**。要在二分查找的过程中，保持不变量，就是在while寻找中每一次边界的处理都要坚持根据区间的定义来操作，这就是**循环不变量**规则。

## 27移除元素

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

~暴力？暴力循环？

~数组肯定不能删除元素 只能覆盖 连续类型临近的 地理位置是删除不了的

~只能前移覆盖前一个 但是空间还是没有变

```
//暴力 双循环
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

erase这个库函数好像也可以删除？ 他不是o1哦   而是on 不要随便用



但是双指针可以on解决 一层for循环做两层for的事情

```
//定义一个快指针 慢指针 都指向nums[0];
//如何操作
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
};//看完视频ac
```

