# day11

对于`+-*/`这些运算符，它们并不是单个字符，而是由两个字符组成的运算符。在C++中，这些运算符是作为字符串处理的，因此应该使用双引号来表示。

例如：

```cpp
string op = "+"; // 表示加法运算符
```

另一方面，`'('`，`'['`，`'{'`，`')'`，`']'`，`'}'` 都是单个字符，因此使用单引号来表示。

## 20有效的括号（经典 栈 技巧）

栈实现队列（两个栈）

队列实现栈（一个即可，出去的进去）

栈的经典应用

建议：大家先自己思考一下 有哪些不匹配的场景，在看视频 我讲的都有哪些场景，落实到代码其实就容易很多了。

怎么把这种有没有效联系到栈的应用？

栈：先进后出，非常适合做对称匹配类的题目。

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char>st;
        for(int i=0;i<s.size();i++){
            if(s[i]=='('){
                st.push(')');
            }
            else if(s[i]=='['){
                st.push(']');
            }
            else if(s[i]=='{'){
                st.push('}');
            }
            else if (st.empty() || st.top() != s[i]) return false;
            //遇到左边的，右边的都进去了，遇到右边的，可以看相不相等。
            else st.pop();//和右边的相等可以推出去。

        }
        return st.empty();//如果栈是空的，就是true。
    }
};
```

![image-20240414201502348](https://cdn.jsdelivr.net/gh/ChristophLevi/AlgorithmPractice@master/image-20240414201502348.png)

## 1047删除字符串中的所有相邻重复

消消乐，消完已有的还要看会不会出新的。小写英文。

```cpp
class Solution {
public:
    string removeDuplicates(string S) {
        stack<char> st;
        for (char s : S) {
            if (st.empty() || s != st.top()) {
                st.push(s);
            } else {
                st.pop(); // s 与 st.top()相等的情况
            }
        }
        string result = "";
        //vector<char>result;
        while (!st.empty()) { //while 栈不为空
            result.push_back(st.top());
            st.pop();
        }
        reverse (result.begin(), result.end()); // 此时字符串需要反转一下
        return result;

    }
};

```

总结：在循环中，当栈为空或者栈顶元素和字符串元素不相等，此时可以压栈，但是只要栈顶元素和字符串元素相等，就把栈顶元素pop了，根本不用管消完之后的事情，因为遇到还会自动消，还要把栈里的元素导出来才能return，用一个string result=“”；然后如果栈不是空的，那么就让result来pushback元素，再pop出栈（先存进result，再pop），最后还要涉及一次reverse，语法是： reverse (result.begin(), result.end()); 

## 150逆波兰表达式求值

引言：就是一种后缀表达式。（方便计算机）

（1+2）x（3+4）这是中缀表达式

后序遍历：左右根 eg：12+34+*

后缀是可以不加括号的，从头到尾就可以计算出结果。

遇到数字：压栈，遇到操作符：1出栈2操作3压栈

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        // 力扣修改了后台测试数据，需要用longlong
        stack<long> st; 
        for (int i = 0; i < tokens.size(); i++) {
            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/") {
                long long num1 = st.top();
                st.pop();
                long long num2 = st.top();
                st.pop();
                if (tokens[i] == "+") st.push(num2 + num1);
                if (tokens[i] == "-") st.push(num2 - num1);
                if (tokens[i] == "*") st.push(num2 * num1);
                if (tokens[i] == "/") st.push(num2 / num1);
            } else {
                st.push(stoll(tokens[i]));
            }
        }

        int result = st.top();
        st.pop(); // 把栈里最后一个元素弹出（其实不弹出也没事）
        return result;
    }
};

```

