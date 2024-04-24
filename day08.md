# day8

## 344反转字符串

建议： 本题是字符串基础题目，就是考察 reverse 函数的实现，同时也明确一下 平时刷题什么时候用库函数，什么时候 不用库函数 

本体就是完全反过来，不能使用库函数的。

面试官一定不是考察你对库函数的熟悉程度。

所以想一想，它和链表的区别是，他是数组，可以直接swap

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
    for(int i=0,j=s.size()-1;i < s.size()/2; i++, j--){
        swap(s[i],s[j]);
    }
    }
};
```

## 541反转字符串II

题目：每2k个字符，反转前k个，都是小写英文。

思路：应该也是swap，swapk个，跳过k个，再swapk个。

reverse的区间应当是s.begin()+i,s.begin()+i+k

但是这个的思想不在swap，可以在满足i+k<=s.size();时直接用reverse。

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
      for(int i=0;i<s.size();i+=2*k){
        if(i+k<=s.size()){
            reverse(s.begin()+i,s.begin()+i+k);
        }
        else{
            reverse(s.begin()+i,s.end());
        }
      }
      return s;
    }
};
```

## 卡码网54替换数字

建议：对于线性数据结构，填充或者删除，后序处理会高效的多。好好体会一下。

题目：输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。                                

输入描述：s字符串，包含小写字母和数字字符

ACM模式！

```
#include <iostream>
#include <cctype>
using namespace std;
int main(){
    string s;
    cin >> s;
    for (char c : s) {
        if (isalpha(c)) {
            cout << c ;
        } else if (isdigit(c)) {
            cout << "number" ;
        } 
    }
}
```

有点取巧啊

先考虑扩充字符串（因为一位数字直接变六个字母）

```cpp
#include <iostream>
using namespace std;
int main() {
    string s;
    while (cin >> s) {
        int sOldIndex = s.size() - 1;
        int count = 0; // 统计数字的个数
        for (int i = 0; i < s.size(); i++) {
            if (s[i] >= '0' && s[i] <= '9') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是将每个数字替换成"number"之后的大小
        s.resize(s.size() + count * 5);
        int sNewIndex = s.size() - 1;
        // 从后往前将数字替换为"number"
        while (sOldIndex >= 0) {
            if (s[sOldIndex] >= '0' && s[sOldIndex] <= '9') {
                s[sNewIndex--] = 'r';
                s[sNewIndex--] = 'e';
                s[sNewIndex--] = 'b';
                s[sNewIndex--] = 'm';
                s[sNewIndex--] = 'u';
                s[sNewIndex--] = 'n';
            } else {
                s[sNewIndex--] = s[sOldIndex];
            }
            sOldIndex--;
        }
        cout << s << endl;       
    }
}
```

有同学问了，为什么要从后向前填充，从前向后填充不行么？

从前向后填充就是O(n^2)的算法了，因为每次添加元素都要将添加元素之后的所有元素整体向后移动。

**其实很多数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作。**

<u>拓展：</u>字符串是字符数组，vector<char>和string 的区别在于string重载了+，所以象处理字符串，一般定义string

## 151反转单词（需要二刷）

leetcode链接：https://leetcode.cn/problems/reverse-words-in-a-string/description/

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果。

建议：这道题目基本把 刚刚做过的字符串操作 都覆盖了，不过就算知道解题思路，本题代码并不容易写，要多练一练。 

```cpp
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }
    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0; 
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }
    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};

```

需要二刷。

## **卡码网55右旋转字符串** 

***注意：reverse都是左闭右开！***

***`reverse(first, last)`会反转从`first`到`last-1`的元素。***

字符串的右旋转操作：后k个字符以到前面。

###### 输入示例

```
2
abcdefg
```

###### 输出示例

```
fgabcde
```

**不能申请额外空间，只能在本串上操作**。

思路：第一段为length-n，第二段为n。

先整体全倒置，再两个子串分别倒置。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;
int main() {
    int n;
    string s;
    cin >> n;
    cin >> s;
    int len = s.size(); //获取长度
    reverse(s.begin(), s.end()); // 整体反转
    reverse(s.begin(), s.begin() + n); // 先反转前一段，长度n
    reverse(s.begin() + n, s.end()); // 再反转后一段

    cout << s << endl;

} 
```

左旋转：

 例如将字符串 "abcdefg" 

向左旋转 3 个字符

得到 "defgabc"。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;
int main() {
    int n;
    string s;
    cin >> n;
    cin >> s;
    int len = s.size(); //获取长度
    reverse(s.begin(), s.begin() + n); //  反转第一段长度为n 
    reverse(s.begin() + n, s.end()); // 反转第二段长度为len-n 
    reverse(s.begin(), s.end());  // 整体反转
    cout << s << endl;

}

```

