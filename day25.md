# day25

## 216组合综合III

如果把 组合问题理解了，本题就容易一些了。 

题目：只用1-9每个数字最多一次，相加之和为n，个数为k，输出的是组合。（二维数组）

本题就是在[1,2,3,4,5,6,7,8,9]这个集合中找到和为n的k个数的组合。

**终止条件：**

```cpp
if (path.size() == k) {
    if (sum == targetSum) result.push_back(path);
    return; // 如果path.size() == k 但sum != targetSum 直接返回
}
```

**本层循环：**

```cpp
for (int i = startIndex; i <= 9; i++) {
    sum += i;
    path.push_back(i);
    backtracking(targetSum, k, sum, i + 1); // 注意i+1调整startIndex
    sum -= i; // 回溯
    path.pop_back(); // 回溯
}
```

**返回值及参数：**

```cpp
vector<vector<int>> result;
vector<int> path;
void backtracking(int targetSum, int k, int sum, int startIndex)
```

最终代码：

```cpp
class Solution {
private:
    vector<vector<int>>result;
    vector<int>path;
    void backtracking(int k,int targetsum,int startindex,int sum){
        for(int i=startindex;i<=9;i++){
            sum+=i;
            path.push_back(i);
            backtracking(k,targetsum,i+1,sum);
            sum-=i;
            path.pop_back();
        }
        if(path.size()==k){
            if(sum==targetsum)result.push_back(path);
        }
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        backtracking(k,n,1,0);
        return result;
    }
};
```

总结：就是终止条件是path的size==k嵌套sum==targetsum就可以pushback进result了；

返回值这道题一定有的是sum和k，n，startindex。

本层循环：for循环套递归，（回溯不涉及序）就是dfs。可以记为：加入--递归i+1--剔除

## 17电话号码的字母组合

本题大家刚开始做会有点难度，先自己思考20min，没思路就直接看题解。 

终止条件：

```cpp
if (index == digits.size()) {
    result.push_back(s);
    return;
}
```

本层循环：

```cpp
int digit = digits[index] - '0';        // 将index指向的数字转为int
string letters = letterMap[digit];      // 取数字对应的字符集
for (int i = 0; i < letters.size(); i++) {
    s.push_back(letters[i]);            // 处理
    backtracking(digits, index + 1);    // 递归，注意index+1，一下层要处理下一个数字了
    s.pop_back();                       // 回溯
}
```

返回值及参数：

```cpp
vector<string> result;
string s;
void backtracking(const string& digits, int index)
```

当处理数字 2 时，对应的字符集为 "abc"。在递归调用中，会依次将 "a"、"b" 和 "c" 逐个添加到结果中，然后对下一个数字进行处理。这样就实现了只将单个字符添加到结果中，而不是一次性添加整个字符集。

```cpp
class Solution {
private:
    const string letterMap[10] = {
        "", // 0
        "", // 1
        "abc", // 2
        "def", // 3
        "ghi", // 4
        "jkl", // 5
        "mno", // 6
        "pqrs", // 7
        "tuv", // 8
        "wxyz", // 9
    };
public:
    vector<string> result;
    string s;
    void backtracking(const string& digits, int index) {
        if (index == digits.size()) {
            result.push_back(s);
            return;//不返回还会出现问题
        }
        int digit = digits[index] - '0';        // 将index指向的数字转为int
        string letters = letterMap[digit];      // 取数字对应的字符集
        for (int i = 0; i < letters.size(); i++) {
            s.push_back(letters[i]);            // 处理
            backtracking(digits, index + 1);    // 递归，注意index+1，一下层要处理下一个数字了
            s.pop_back();                       // 回溯
        }
    }
    vector<string> letterCombinations(string digits) {
        s.clear();
        result.clear();
        if (digits.size() == 0) {
            return result;
        }
        backtracking(digits, 0);
        return result;
    }
};
```

