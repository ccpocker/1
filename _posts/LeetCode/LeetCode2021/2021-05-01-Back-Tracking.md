---
layout: post
title:  "BackTracking Review"
categories: LeetCode
tags: BackTracking
author: ccpocker
---

* content
{:toc}

## 综述
BackTracking 称之为回溯法。回溯法本质上是一种穷举，穷举所有可能，然后找到我们所需要的答案。

回溯法解决的问题：

- 组合问题：
- 切割问题:
- 子集问题:
- 排列问题:
- 棋盘问题:


### 回溯法的模板
每个回溯算法都可以抽象为一个N叉树的搜索，集合的大小构建了树的宽度，递归的深度构成了树的深度。参考图:figure1

![figure1](https://camo.githubusercontent.com/f65ca647f31913496481cd1aff144040bd7ee4f6bc30accd370bc78b4b265d13/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303231303133303137333633313137342e706e67)

```cpp
void backTracking(参数){
    //终止条件
    if(终止条件){
        存放结果;
        return;
    }
    //回溯搜索的遍历过程
    for(选择:本层集合中的元素(树中结点孩子的数量就是集合的大小)){//横向遍历
        处理节点;
        backTracking(路径,选择列表); //递归,纵向遍历
        回溯,撤销处理结果
    }
}
```



### 组合问题

#### [LeetCode 77 组合](https://leetcode-cn.com/problems/combinations/)

**组合是无序的**,例如[1,2]和[2,1]是同一个组合;

```cpp
//leetcode 77. 组合
void backTracking77(vector<vector<int>> &res,vector<int> &cur,int n,int k,int start){
    if(cur.size()==k){
        res.emplace_back(cur);
        return;
    }
    //可剪枝优化,for(int i=start;i<=n-(k-path.size())+1;++i)
    for(int i=start;i<=n;++i){ 
        
        cur.push_back(i);
        backTracking77(res,cur,n,k,i+1);
        cur.pop_back(); //撤销
    }

}
vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> res;
    vector<int> cur;
    backTracking77(res,cur,n,k,1);
    return res;
}
```


#### [LeetCode 216.组合总和Ⅲ](https://leetcode-cn.com/problems/combination-sum-iii/)

```cpp
void backTracking216(vector<vector<int>> &res,vector<int> &cur,int n,int k,int start){
    if(k==0){
        if(n==0) res.emplace_back(cur);
        return;
    }

    for(int i=start;i<=min(9,n);++i){
        cur.push_back(i);
        backTracking216(res,cur,n-i,k-1,start=i+1);
        cur.pop_back();
    }

}

vector<vector<int>> combinationSum3(int k, int n) {
    vector<vector<int>> res;
    vector<int> cur;
    backTracking216(res,cur,n,k,1);
    return res;
}

```

#### [LeetCode 17.电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/submissions/)

```cpp
vector<string> vec_str17{"abc","def","ghi","jkl","mno","pqrs","tuv","xyz"};
void backTracking17(vector<string> &res,string curStr,string digits,int start)
{
    if(start==digits.size()){
        res.emplace_back(curStr);
        return;
    }


    char c=digits[start];
    if (c!='1'&&c!='0'){
        string temp=vec_str17[c-'2'];
        for(char c:temp){
            curStr+=c;
            backTracking17(res,curStr,digits,start+1);
            curStr.pop_back();
        }
    }
    
}

vector<string> letterCombinations(string digits) {
    vector<string> res;
    if(digits.empty()||(digits.size()==0&&digits=="1"))
        return res;
    string curStr;
    backTracking17(res,curStr,digits,0);
    return res;

}

```

#### [LeetCode 39](https://leetcode-cn.com/problems/combination-sum-i/)
```cpp
//LeetCode 39.组合总和I
void backTracking39(vector<vector<int>> &res,vector<int> &cur,vector<int> &candidates,int n,int start){
    if(n==0){
        res.emplace_back(cur);
        return;
    }
    for(int i=start;i<candidates.size()&&candidates[i]<=n;++i){
        cur.push_back(candidates[i]);
        backTracking39(res,cur,candidates,n-candidates[i],i);
        cur.pop_back();
    }

}

vector<vector<int>> combinationSum(vector<int>& candidates, int target){
    vector<vector<int>> res;
    vector<int> cur;
    sort(candidates.begin(),candidates.end());
    backTracking39(res,cur,candidates,target,0);
    return res;
}

```
#### [LeetCode 39](https://leetcode-cn.com/problems/combination-sum-i/) 
```cpp
//LeetCode 40.组合总和II
/*
去重使用一个used数组，同一树层级使用过的数字不再使用，但同分支的相同数字是可以继续使用的。
*/
void backTracking39(vector<vector<int>> &res,vector<int> &cur,vector<int> &candidates,vector<bool> &used,int n,int start){
    if(n==0){
        res.emplace_back(cur);
        return;
    }
    for(int i=start;i<candidates.size()&&candidates[i]<=n;++i){
        if(i>0&&candidates[i]==candidates[i-1]&&!used[i-1]){
            continue;
        }else{
            cur.push_back(candidates[i]);
            used[i]=true;
            backTracking39(res,cur,candidates,used,n-candidates[i],i+1);
            used[i]=false;
            cur.pop_back();
        }
    }

}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target){
    vector<vector<int>> res;
    vector<bool> used(candidates.size(),false);
    vector<int> cur;
    sort(candidates.begin(),candidates.end());
    backTracking39(res,cur,candidates,used,target,0);
    return res;
}
```