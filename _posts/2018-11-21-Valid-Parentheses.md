---
layout: post
title:  LeetCode 20.Valid Parenthese
categories: LeetCode
tags: 每日一题 string stack 
author: ccpocker
---

* content
{:toc}

## [LeetCode20.Valid Parenthese](https://leetcode.com/problems/valid-parentheses/)
思路:使用一个栈便可解决。遍历整个字符串:如果是'('、'['、'{',就将其压栈。如果是')'、']'、'}',就从栈弹出一个元素，如果栈为空或者弹出的元素与遍历到的字符不匹配，就返回false。

C++代码
```C++
第一种方式:0ms
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        for(char& c:s){
            switch(c){
                case '(':stk.push(')');break;
                case '[':stk.push(']');break;
                case '{':stk.push('}');break;
                default:
                    if(stk.empty()||c!=stk.top()) return false;
                    else stk.pop();
            }
        }
        return stk.empty();
        
    }
};
第二种方式:4ms
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        for(char c:s){
            switch(c){
                case '(':stk.push(')');break;
                case '[':stk.push(']');break;
                case '{':stk.push('}');break;
                default:
                    if(stk.empty()||c!=stk.top()) return false;
                    else stk.pop();
            }
        }
        return stk.empty();
        
    }
}；
```
这两种方式唯一的区别就是遍历的方式：
第一种使用的是for(char& c:s):这种方式是直接在原字符串进行遍历操作
第二种使用for(char& c:s):这种方式有一个复制原字符串过程、故耗时一些。

python代码
```python
class Solution:
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stk=[]
        dic= {"]":"[", "}":"{", ")":"("}
        for ch in s:
            if ch in dic.values():
                stk.append(ch)
            elif ch in dic.keys():
                if stk==[] or stk.pop()!=dic[ch]:
                    return False
            else:
                return False
        return stk==[]
```
合理利用Python内建的列表、字典，能有效的简化程序。
