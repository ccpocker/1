---
layout: post
title:  LeetCode 38. Count and Say
categories: LeetCode
tags: 每日一题 string
author: ccpocker
---

* content
{:toc}

## 38. Count and Say
[38. Count and Say](https://leetcode.com/problems/count-and-say/)
| n | out|
| - | :-: |
| 1 | 1 |
| 2 | 11 |
| 3 | 21 |
| 4 | 1211|
| 5 | 111221|

刚开始做这题的时候，很迷糊。查了一些资料，题意看懂了就比较简单。当n=1，输出就是‘1’，从n=2开始，每个输出就是上个输出的**一种描述方式**。例如当n=2，由于n=1,out1=1，那么out2的描述就是1个1，所以out2=11.n=3时，out2=12,那么out3的描述就是2个1，所以out3=21.同理out4=1211.依次类推。

编码的思路就和上述思考是一致的。
#### C++
```C++
class Solution {
public:
    string countAndSay(int n) {
        if(n<=0) return "";
        string curr="1";
        while(--n){
            string next="";
            for(int i=0;i<curr.size();i++){
                int count=1; //出现次数
                while(i+1<curr.size()&&curr[i]==curr[i+1]){
                    count++;
                    i++;
                }
                next+=to_string(count)+curr[i];
            }
            curr=next;

        }
        return curr;
    }
};
```
#### Python


```Python
class Solution:
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        s = '1'
        for _ in range(n - 1):
            s = ''.join(str(len(list(group))) + digit
                        for digit, group in itertools.groupby(s))
        return s
```
