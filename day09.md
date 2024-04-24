# day9

## 28实现strStr（）（只看了思路）

### 理论基础：

<u>kmp思想：</u>当出现字符串不匹配时，可以记录一部分之前已经匹配的文本内容，用来避免暴力从头循环匹配。

把o（mn）简化为o（m+n）

<u>如何记录已经匹配的内容：</u>next数组

<u>面试官问：</u>**next数组里的数字表示的是什么？**

**为什么这么表示？**

<u>答案：</u>next数组是一个前缀表，用来回退，记录了模式串和主串如果不匹配，模式串要从哪里开始重新匹配。

<u>*实例：*</u>要在文本串：aabaabaafa 中查找是否出现过一个模式串：aabaaf。

<u>next数组（前缀表）：</u>**记录下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。**

<u>***前缀是指不包含最后一个字符的所有以第一个字符开头的连续子串。***</u>

<u>***后缀是指不包含第一个字符的所有以最后一个字符结尾的连续子串。***</u>

aabaaf的前缀是a aa aab aaba aabaa不包含尾字母

aabaaf的后缀是f af aaf baaf abaaf不包含首字母

”最长相等前后缀“

a 0

aa 1

aab 0

aabaa 2

aabaaf 0就是最长相等前后缀，得到了前缀表next数组

所以如果再aabaa的后面那个f开始不匹配了，就找到aabaa的最后一个a那里看他的next表值，得到是2，就从数组的下标为2的b重新开始匹配。

### 具体实现：

```cpp
class Solution {
public:
    void getNext(int* next, const string& s) {
        int j = -1;
        next[0] = j;
        for(int i = 1; i < s.size(); i++) { // 注意i从1开始
            while (j >= 0 && s[i] != s[j + 1]) { // 前后缀不相同了
                j = next[j]; // 向前回退
            }
            if (s[i] == s[j + 1]) { // 找到相同的前后缀
                j++;
            }
            next[i] = j; // 将j（前缀的长度）赋给next[i]
        }
    }
    int strStr(string haystack, string needle) {
        if (needle.size() == 0) {
            return 0;
        }
		vector<int> next(needle.size());
		getNext(&next[0], needle);
        int j = -1; // // 因为next数组里记录的起始位置为-1
        for (int i = 0; i < haystack.size(); i++) { // 注意i就从0开始
            while(j >= 0 && haystack[i] != needle[j + 1]) { // 不匹配
                j = next[j]; // j 寻找之前匹配的位置
            }
            if (haystack[i] == needle[j + 1]) { // 匹配，j和i同时向后移动
                j++; // i的增加在for循环里
            }
            if (j == (needle.size() - 1) ) { // 文本串s里出现了模式串t
                return (i - needle.size() + 1);
            }
        }
        return -1;
    }
};

```

## 459重复的子字符串

```cpp
class Solution {
public:
    void getNext (int* next, const string& s){
        next[0] = 0;
        int j = 0;
        for(int i = 1;i < s.size(); i++){
            while(j > 0 && s[i] != s[j]) {
                j = next[j - 1];
            }
            if(s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern (string s) {
        if (s.size() == 0) {
            return false;
        }
        int next[s.size()];
        getNext(next, s);
        int len = s.size();
        if (next[len - 1] != 0 && len % (len - (next[len - 1] )) == 0) {
            return true;
        }
        return false;
    }
};

```

## 字符串总结

### 什么是字符串

那么vector< char > 和 string 又有什么区别呢？

其实在基本操作上没有区别，但是 string提供更多的字符串处理的相关接口，例如string 重载了+，而vector却没有。

所以想处理字符串，我们还是会定义一个string类型。

### 要不要用库函数

关键部分可以解决就不要用

一般是reverse

### 双指针法

双指针法可以用来反转字符串，双指针法在数组链表字符串中非常常用

### 反转系列

反转字符串ii可以 i += (2 * k)解决，可以用reverse

## kmp算法

匹配问题

重复子串问题

## 双指针法大总结

### 数组篇

所以此时使用双指针法才展现出效率的优势：**通过两个指针在一个for循环下完成两个for循环的工作。**

### 字符串篇

### 链表篇

### N数之和篇

（待更新）
