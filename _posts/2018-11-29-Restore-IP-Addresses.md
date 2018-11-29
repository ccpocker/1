---
layout: post
title:  LeetCode 93. Restore IP Addresses
categories: LeetCode
tags: 每日一题 string backtracking
author: ccpocker
---

* content
{:toc}

## [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

题目描述：给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式.

IP地址是一个四段数，每段的的数为0-255，例如0.0.0.0或255.255.255.2但是不能有01这样的数出现。

思路：


C++
```cpp
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vecotr<string> res;
        dfs(s,0,0,"",res);
        return res;
    }
    //string 待重建IP的string
    //start: 重建的开始位置
    //index:已经重建好的部分数量（简单理解，已经打了几个点）
    //path：重建好的string
    void buildIP(string s,int start,int index,string path,vector<string> res){
        if(start==s.size()&&index==4){
            path.erase(ip.end()-1); //减去最后那个.
            res.push_back(path);
        }

        if(s.size()-start>(4-step)*3) return; //剩余过多
        if(s.size()-start<(4-step)) return;

        int num=0;
        for(int i=start,i<start+3;i++){
            num=num*10+(s[i]-'0');
            if(num<=255)
                ip+=s[i];
                buildIp(s,i+1,step+1,ip+'.',res);
        }
        if(num==0) break; //首位是0,跳出循环。
        

    }
};

```

```python
class Solution:
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        res=[]
        self.dfs(s,0,"",res)
        return res
    
    def dfs(s,index,path,res):
        if not s and index==4:
            res.append(path)
            return
        for i in range(1,4):
            if i<=len(s):
                if int(s[:i])<=255:
                    self.dfs(s[i:],index+1,path+s[:i],res)
                if s[0]="0":
                    break
```