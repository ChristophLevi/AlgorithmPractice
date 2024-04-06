# day4

## 24两两交换链表节点

两两交换 返回交换后的头节点

想到递归 迭代 递归的空间一般是on 迭代是o1（不额外）

![image-20240406205934264](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240406205934264.png)

递归的思想：我最终还是要的是从第二个节点开始 

所以第二个是newhead

递归节点自然成了swappair（newhead->next)

注意先让head指向递归节点，再让newhead指向head

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(!head || !head->next) {
            return head;
        }//终止条件
        ListNode* newhead = head->next;
        head->next = swapPairs(newhead->next);
        newhead->next = head;
        return newhead;
    }
};
```

迭代：

![image-20240406213744083](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240406213744083.png)

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode(0); 
        dummy->next = head; 
        ListNode* cur = dummy;

        while(cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* first = cur->next;
            ListNode* second = cur->next->next;
            // 节点交换
            cur->next = second;
            first->next = second->next;
            second->next = first;
            cur = second->next; 
        }
        head=dummy->next;
        delete dummy;
        return head;
    }
};
```

迭代的思想：虚节点 第一节点 第二节点

三次改换next操作，循环动起来全靠虚节点的：

下一个和下下一个不是nullptr以及cur指向下下下一个

（重要的是不动head 免得被绕晕）

## 19删除倒数第n节点

题目：链表倒数第n个给他删了

思路：删除肯定需要知道前一个节点，用于改next指向

找到倒数第n个？快慢指针：

快的先跑n+1，快慢一起跑，他们必定相距n

直到快等于nullptr，慢就是倒数第n个前一个，好操作了

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode*dummy=new ListNode(0);
        dummy->next=head;
        ListNode*fast=dummy;
        ListNode*slow=dummy;
        for(int i=0;i<n+1;i++){
            fast=fast->next;
        }
        while(fast!=nullptr){
            slow=slow->next;
            fast=fast->next;
        }
        slow->next=slow->next->next;
        return dummy->next;
    }
};


```

代码写的很舒服，思路是对的就很快

## 面试题 链表相交

题目：让长的和短的链表一块比 看到啥时候“指针相等”而不是值相等！！！

指向的是同一个内存地址，那么它们就是相等的。

思路：长的先走到和短同步的位置，再一块对比指针

涉及到链表的length，遍历求解。

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode *tempA = headA;
    ListNode *tempB = headB;
    int lenA = 0, lenB = 0;
// 计算链表headA的长度
    while (tempA != nullptr) {
        lenA++;
        tempA = tempA->next;
    }
// 计算链表headB的长度
    while (tempB != nullptr) {
        lenB++;
        tempB = tempB->next;
    }
    tempA = headA;
    tempB = headB;
// 让tempA和tempB分别指向两个链表的起始节点
// 并让长链表的指针先向前移动差值步
    if (lenA > lenB) {
        for (int i = 0; i < lenA - lenB; i++) {
            tempA = tempA->next;
        }
    } else {
        for (int i = 0; i < lenB - lenA; i++) {
            tempB = tempB->next;
        }
    }
// 同时移动tempA和tempB，直到它们相遇
    while (tempA != tempB) {
        tempA = tempA->next;
        tempB = tempB->next;
    }
    return tempA; // 返回相交的起始节点，如果没有相交节点返回nullptr
}
};
```

## 142环形链表II

输出的是有环的元素的下标

找环的入口下标元素 没有环返回0

思路：快慢双指针，快的一次两步，慢的一次一步，有环会相遇的，相遇之后，再一个finder从头出发，慢也出发，finder遇到slow直接返回下标

为什么有环，快慢指针会相遇？

答案：想象在操场跑步吧，秒了

为什么finder和slow相遇在环的开头？

![image-20240406233957749](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240406233957749.png)

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode*slow=head;
        ListNode*fast=head;
        ListNode*finder=head;
while(fast&&fast->next){
            fast=fast->next->next;
            slow=slow->next;
            if(slow==fast){
             while(finder!=slow){
                finder=finder->next;
                slow=slow->next;
            }
            return finder;
            }
            }
            return nullptr;
           
        }
};
```

