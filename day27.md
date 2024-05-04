# day27

## 39组合总和

本题是 集合里元素可以用无数次，那么和组合问题的差别 其实仅在于 startIndex上的控制

```cpp
class Solution {
private:
    vector<vector<int>>result;
    vector<int>path;
    void backtracking(vector<int>&candidates,int target,int sum,int startIndex){
         for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) {
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum, i); 
            sum -= candidates[i];
            path.pop_back();
        }
        if(sum==target){
            result.push_back(path);
            return;
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    backtracking(candidates,target,0,0);
    return result;
    }
};
```

> [!IMPORTANT]
>
> 注意：需要剪枝： i < candidates.size() && sum + candidates[i] <= target; 
>
> 如果不剪 可能超时，但是这个剪枝的前提时：必须sort(candidates.begin(), candidates.end());（先有序）

## 40组合总和ii

本题开始涉及到一个问题了：去重。

注意题目中给我们 集合是有重复元素的，那么求出来的 组合有可能重复，但题目要求不能有重复组合。 

```cpp
class Solution {
private:
    vector<vector<int>>result;
    vector<int>path;
    void backtracking(vector<int>&candidates,int target,int sum,int startIndex){
         for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++){
             if (i >0&&candidates[i]==candidates[i-1]) 
                 {
                return;
                 }//去重操作 去重操作 去重操作 去重操作 去重操作 去重操作 去重操作
            sum+=candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates,target,sum,i); 
            sum-=candidates[i];
            path.pop_back();
        }
        if(sum==target){
            result.push_back(path);
            return;
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    sort(candidates.begin(),candidates.end());
    backtracking(candidates,target,0,0);
    return result;
    }
};
```

## 131分割回文串

本题较难，大家先看视频来理解 分割问题，明天还会有一道分割问题，先打打基础。 

把字符串分割成回文串，**回文串是向前和向后读都相同的字符串。**

等二刷。