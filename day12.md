# day12

## 239滑动窗口最大值

**题目又叫：程序员到点被pop，（队头pop 光荣）**

**但比新人垃圾的不到点也被pop（队尾pop 被刷了）**

之前讲的都是栈的应用，这次该是队列的应用了。

本题算比较有难度的，需要自己去构造单调队列，建议先看视频来理解。 

难点：如何求窗口内最大值？多循环？o（nk）了

单调队列法：维护队列里的单调递增或者单调递减

伪代码：

```cpp
队列出口是front 入口时back
在que函数中
deque<int>que;
pop (int val){
    if(!que.empty()&&val==que.front()){
        que.pop_front();
    }
}
push(int val){
    while(!que.empty()&&val>que.back()){
        que.pop_back();
}
```

先弄k个元素push进去，暗箱操作，获得队头。（0--k-1）

再弄个循环。

能够不断推进的循环：循环从i=k到i小于nums.size()

在循环中，pop掉i-k位 push进新的 在把表头加入result

```cpp
class Solution {
private:
    class MyQueue { 
    public:
        deque<int> que; 
        void pop(int value) {
            if (!que.empty() && value == que.front()) {
                que.pop_front();
            }//这个pop 只会使滑动窗口右移的感觉 并不会pop别的
        }//pop的规则时 如果元素等于出口元素，那我出口可以直接pop了。
        void push(int value) {
            while (!que.empty() && value > que.back()) {
                que.pop_back();
            }
            que.push_back(value);
        }//加入元素 首先如果我比back大，就先pop了 pop干净小崽子，再push_back;
        int maxval() {
            return que.front();
        }//最大就是出口处元素（也是front）
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> result;
        for (int i = 0; i < k; i++) { // 前k个入队
            que.push(nums[i]);
        }
        result.push_back(que.maxval()); // result 记录前k的元素的最大值
        for (int i = k; i < nums.size(); i++) {
            que.pop(nums[i - k]); // 滑动窗口移除最前面元素
            que.push(nums[i]); // 滑动窗口前加入最后面的元素
            result.push_back(que.maxval()); // 记录对应的最大值
        }
        return result;
    }
};

```

因为我滑动要消亡的是nums【i-k】 并不一定是队头 

## 347前 K 个高频元素（需二刷）

```cpp
class Solution {
public:
    // 小顶堆
    class mycomparison {
    public:
        bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
            return lhs.second > rhs.second;
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        // 要统计元素出现频率
        unordered_map<int, int> map; // map<nums[i],对应出现的次数>
        for (int i = 0; i < nums.size(); i++) {
            map[nums[i]]++;
        }

        // 对频率排序
        // 定义一个小顶堆，大小为k
        priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;

        // 用固定大小为k的小顶堆，扫面所有频率的数值
        for (unordered_map<int, int>::iterator it = map.begin(); it != map.end(); it++) {
            pri_que.push(*it);
            if (pri_que.size() > k) { // 如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                pri_que.pop();
            }
        }

        // 找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒序来输出到数组
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--) {
            result[i] = pri_que.top().first;
            pri_que.pop();
        }
        return result;

    }
};

```

## 栈与队列总结

1. C++中stack，queue 是容器么？

   不是 是容器适配器 使得容器在某些语义下更好用

2. 我们使用的stack，queue是属于那个版本的STL？

   c++98

3. 我们使用的STL中stack，queue是如何实现的？

   栈一般是deque或vector

   queue一般是deque或者list

4. stack，queue 提供迭代器来遍历空间么？

   适配器没有迭代器，只有铺push pop op

   front back

   面试题：栈里面的元素在内存中是连续分布的么？

   这个问题有两个陷阱：

   - 陷阱1：栈是容器适配器，底层容器使用不同的容器，导致栈内数据在内存中不一定是连续分布的。

   - 陷阱2：缺省情况下，默认底层容器是deque，那么deque在内存中的数据分布是什么样的呢？ 答案是：不连续的，下文也会提到deque。

     

     `queue` 和 `deque` 在 C++ 中是两种不同的容器，它们有一些区别：

     1. 数据结构：`queue` 是一种队列，它是一种先进先出（FIFO）的数据结构，只允许在队尾插入元素，在队头删除元素。而 `deque` 是双端队列，允许在队头和队尾进行插入和删除操作。
     2. 实现：`queue` 通常基于 `deque` 或 `list` 实现，而 `deque` 是一种双端队列，内部使用分段连续内存实现，支持快速的随机访问和在两端的插入和删除操作。
     3. 接口：`queue` 提供了队列的操作，如 `push`（在队尾插入元素）、`pop`（删除队头元素）、`front`（访问队头元素）、`back`（访问队尾元素）等。而 `deque` 提供了双端队列的操作，如 `push_back`（在队尾插入元素）、`push_front`（在队头插入元素）、`pop_back`（删除队尾元素）、`pop_front`（删除队头元素）等。

### 栈的应用stack

1.cd系统目录

2.括号匹配。

3.消消乐

4.后缀表达式处理

### 队列的应用queue

1.滑动窗口最大值

2.前k个高频元素

