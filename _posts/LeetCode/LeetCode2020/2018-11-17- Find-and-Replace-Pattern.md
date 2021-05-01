---
layout: post
title:  LeetCode 890. Find and Replace Pattern
categories: LeetCode
tags: 每日一题 string
author: ccpocker
---

* content
{:toc}

## 890. Find and Replace Pattern
题目描述：给出一个字符串列表和一个字符串，从列表中找出与给定字符串相同模式的字符串。
[890. Find and Replace Pattern](https://leetcode.com/problems/find-and-replace-pattern/)

思路：利用字典，将每个字符创转成同样的模式(以a为第一次出现的字母）。如：bbccdd->aabbcc，xxyyzz->aabbcc。

```cpp
class Solution {
public:
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        vector<string> res;
        string pd=toPattern(pattern);
        for(string& w:words){
            if(toPattern(w).compare(pd)==0)
                res.push_back(w);
        }
        return res;
        
    }
    //将字符串转成同一模式
    string toPattern(string word){
        unordered_map<char,char> M;
        int curr=97;
        for(char&c: word) {
            if(M.count(c)==0) 
                M[c]=(char)curr++;
        }
        for(int i=0;i<word.size();i++){
            word[i]=M[word[i]];
        }
        return word;
    }
};
```

```python
# dict.setdefault(key,default=None)
# 如果键不在字典中，就会添加键并将值设为default
class Solution:
    def findAndReplacePattern(self, words, pattern):
        """
        :type words: List[str]
        :type pattern: str
        :rtype: List[str]
        """
        def toPattern(word):
            dic={}
            return [dic.setdefault(c,len(dic)) for c in word]
        return [w for w in words if toPattern(w)==toPattern(pattern)]

```