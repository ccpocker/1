---
layout: post
title:  第一次LeetCode Contest
categories: LeetCode Contest
tags: Array, Stack
author: ccpocker
---


* content
{:toc}

### 前言
第一次参加LeetcodeContest，总共四道题目，限时一个半小时。AC两道:可惜多次提交，罚了太多时间，所以经验就是可以自己在IDE进行调试，保证正确后再提交。

牛人真的太多了，我这个渣渣就多去感受感受。参与第一，比赛第二。不过赛后总结还是要写写，积累经验。

#### [945. Minimum Increment to Make Array Unique](https://leetcode.com/problems/minimum-increment-to-make-array-unique/description/)

***题目描述***：给定一个整数列表。每次可以对一个元素进行+1，这个操作叫做一次move。那么最少多少次move，使这个列表内的每个元素都是唯一的。

***思路***：先对整个数组进行排序。然后遍历，如果pre>=curr，先对res=pre-curr+1，然后令curr=pre+1。最后返回res

```cpp
class Solution {
public:
    int minIncrementForUnique(vector<int>& A) {
        sort(A.begin(),A.end());
        int res=0;
        for(int i=1;i<A.size();i++){
            if(A[i]>A[i-1])
                continue;
            else
                res+=A[i-1]-A[i]+1;
                A[i]=A[i-1]+1;
        }
        return res;
    }
};
```

```python
class Solution:
    def minIncrementForUnique(self, A):
        """
        :type A: List[int]
        :rtype: int
        """
        if not A:
            return 0
        A.sort()
        ret,pre=0,A[0]
        for x in A[1:]:
            if x<=pre：
                ret+=pre-x+1
                pre+=1
            else:
                pre=x
        return ret

```


#### [946. Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/description/)

***题目描述***:给出两个序列(序列的每个元素都是不同的)，然后一个判断后一个序列能否由前一个序列和一个栈得到。

>Example 1:
**Input**: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
**Output**: true
**Explanation**: We might do the following sequence:
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
Example 2:
**Input**: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
**Output**: false
**Explanation**: 1 cannot be popped before 2.

***思路***：将序列pushed压栈直到栈顶为序列popped的第一个元素

```cpp
class Solution {
public:
    //自己写的丑陋版本，调试了很久才出来
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> s1;
        int i=0;
        int j=0;
        while(j<popped.size()){
            while(s1.empty()||s1.top()!=popped[j])
                s1.push(pushed[i++]);
            while(s1.empty()&&s1.top()==poped[j]){
                s1.pop();
                j++;
            }
            if(i=pushed.size()&& j<popped.size() && s1.top()!=poped[j])
                return false;
        }
        return true;
        
        
    }
};

    //别人的简洁代码
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> s;
        int i = 0;
        for (int x : pushed) {
            s.push(x);
            while (s.size() && s.top() == popped[i]) {
                s.pop();
                i++;
            }
        }
        return i == popped.size();
    }
```