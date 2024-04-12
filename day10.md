# day10

## 栈 队列 基础

栈是先进后出，队列啊是先进先出。

栈和队列都是stl里面的数据结构。

三个最为普遍的STL版本：

1. HP STL 其他版本的C++ STL，一般是以HP STL为蓝本实现出来的，HP STL是C++ STL的第一个实现版本，而且开放源代码。
2. P.J.Plauger STL 由P.J.Plauger参照HP STL实现出来的，被Visual C++编译器所采用，不是开源的。
3. SGI STL 由Silicon Graphics Computer Systems公司参照HP STL实现，被Linux的C++编译器GCC所采用，SGI STL是开源软件，源码可读性甚高。

栈和队列是SGI STL里面的数据结构。

“栈只有push和pop而没有走访的迭代器。

**可以控制使用哪种容器来实现栈的功能**。”

因此：栈不是容器，栈是容器适配器。（因为没有迭代器）

栈用什么容器呢？无所谓，可以是vector，**deque**，list。

默认是双向队列的deque，因为封住一端就可以实现栈。

我们也可以指定vector为栈的底层实现，初始化语句如下：

```cpp
std::stack<int, std::vector<int> > third;  // 使用vector为底层容器的栈
```

也可以指定list 为起底层实现，初始化queue的语句如下：

```cpp
std::queue<int, std::list<int>> third; // 定义以list为底层容器的队列
```

## 232用栈实现队列

思路：因为栈是限制一端，恰好和队列出元素相反，我们再用一个栈，满足出栈与出队列成为同样的元素了。（但要满足第一个栈全弹进第二个栈，不然会乱序）

```cpp
#include <stack>

class MyQueue {
public:
    stack<int> stackIn;
    stack<int> stackOut;

    MyQueue() {

    }
    void push(int x) {
        stackIn.push(x);
    }

    int pop() {
        if(stackOut.empty()){
            while(!stackIn.empty()){
                stackOut.push(stackIn.top());
                stackIn.pop();
            }
        }
        int result = stackOut.top();
        stackOut.pop();
        return result;
    }

    int peek() {
        int res = this->pop(); // 直接使用已有的pop函数
        stackOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
        return res;
    }

    bool empty() {
        return stackIn.empty() && stackOut.empty();
    }
};

```

1.要注意的是push的操作，push到in栈即可。

2.pop的操作，需要out为空，in不为空，out里把in的top开始全部吸进去，再把in的pop出去，最后再把result当作out的top，慢慢pop。

3.peek是要返回队列开头的元素，

```cpp
int peek() {
        int res = this->pop(); // 直接使用已有的pop函数
        stackOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
        return res;
    }
```

## 225用队列实现栈

队列：先入先出------栈：先入后出

实际上：用一个队列就可以实现栈了，和栈实现队列不一样

```cpp
class MyStack {
public:
    queue<int> que;
    /** Initialize your data structure here. */
    MyStack() {

    }
    /** Push element x onto stack. */
    void push(int x) {
        que.push(x);
    }
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int size = que.size();
        size--;
        while (size--) { // 将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部
            que.push(que.front());
            que.pop();
        }
        int result = que.front(); // 此时弹出的元素顺序就是栈的顺序了
        que.pop();
        return result;
    }

    /** Get the top element. */
    int top() {
        return que.back();
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return que.empty();
    }
};

```

