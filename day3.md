# day3

## 链表基础

单链 双链 循环链

### 手写链表

```c++
class ListNode{
    public:
    int val;//元素
    ListNode *next;
    ListNode(int x):val(x),next(nullptr){}
};
```

或者自定义构造函数初始化节点

```
ListNode*head=new ListNode(5);
```

### 链表操作

删除：前一个next指向后一个 再delete中间的

添加：新-》后  前-》新

插入删除：数组为on 链表为01

查询：数组为01  链表为on

## 203移除链表元素

建议： 本题最关键是要理解 虚拟头结点的使用技巧，这个对链表题目很重要。

要求删除所有的val元素

删除肯定牵扯到前中后 所以需要dummy（虚拟头节点）

怎么处理dummy 头节点 后一节点？

cur temp 自增的是cur就可以

为什么不可以是dummy遍历链表：因为指定他是虚拟头

而且最后也不能返回head了 返回dummy->next才对

但是更极致：需要让dummy的next为head-delete dummy

temp也需要delete 但是不在外面 再本循环中 

并且最后必须是head=dummy->next;而非定义语句（重复）

```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode*dummy=new ListNode(0);
        dummy->next=head;
        ListNode*cur=dummy;
        if(head==nullptr){
            return nullptr;
        }
        while(cur->next!=nullptr){
if(cur->next->val==val){
    ListNode*temp=cur->next;
    cur->next=temp->next;delete temp;
}else{
    cur=cur->next;
}
        }head=dummy->next;
        delete dummy;
        return head;
    }
};

```

![image-20240405205611937](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240405205611937.png)

## 707设计链表

重点练习虚拟头节点

```cpp
MyLinkedList()
int get(int index)
void addAtHead(int val)
void addAtTail(int val)
void addAtIndex(int index, int val)
void deleteAtIndex(int index)
```

获取第n值

头插

尾插

n节点前插

删第n个

```cpp
class MyLinkedList {
public:
    class ListNode{
    public:
        int val;
        ListNode* next;
        ListNode(int x):val(x),next(nullptr){}
    };
    ListNode* dummy;
    ListNode* head;
    
    MyLinkedList() {
        dummy = new ListNode(0);
        head = nullptr;
        dummy->next = head;
    }
    
    int get(int index) {
        ListNode* cur = dummy->next; 
        for (int i = 0; i < index; i++) {
            if (cur == nullptr) {
                return -1;
            }
            cur = cur->next;
        }
        return cur != nullptr ? cur->val : -1;
    }
    
    void addAtHead(int val) {
        ListNode* newnode = new ListNode(val);
        newnode->next = dummy->next;
        dummy->next = newnode;
        if (head == nullptr) {
            head = newnode;
        }
    }
    
    void addAtTail(int val) {
        ListNode* newnode = new ListNode(val);
        ListNode* cur = dummy;
        while (cur->next != nullptr) {
            cur = cur->next;
        }
        cur->next = newnode;
        if (head == nullptr) {
            head = newnode;
        }
    }
    
    void addAtIndex(int index, int val) {
        if (index < 0) {
            return;
        }
        if (index == 0) {
            addAtHead(val);
            return;
        }
        ListNode* newnode = new ListNode(val);
        ListNode* cur = dummy;
        for (int i = 0; i < index; i++) {
            if (cur->next == nullptr) {
                return;
            }
            cur = cur->next;
        }
        newnode->next = cur->next;
        cur->next = newnode;
    }
    
    void deleteAtIndex(int index) {
        ListNode* cur = dummy;
        for (int i = 0; i < index; i++) {
            if (cur->next == nullptr) {
                return;
            }
            cur = cur->next;
        }
        if (cur->next != nullptr) {
            ListNode* temp = cur->next;
            cur->next = temp->next;
            delete temp;
        }
    }
};

```

方法比题解简单 易懂

## 206反转链表

自己想想 这种直接完全反转过来的 好像可以迭代 递归

递归应该很简单 不用dummy cur temp空间on

递归空间o1

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = nullptr;
        ListNode* temp = head;
        while (temp) {
            ListNode* nextnode = temp->next;
            temp->next = cur;
            cur = temp;
            temp = nextnode;
        }
        delete temp;
        return cur;
    }
};

```

迭代：

![image-20240405224511988](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240405224511988.png)

递归：

![image-20240405225438210](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240405225438210.png)