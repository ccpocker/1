---
layout: post
title:  第一次LeetCode Contest
categories: LeetCode Contest
tags: Array, Stack
author: ccpocker
---

* content
{:toc}

### 第二次Leetcode contest

#### [949. Largest Time for Given Digits]()

题目描述:给你一个一个数组，其中包含四个数字。然后返回用这四个数字返回表示时间最大的字符串。

```python
class Solution:
    def largestTimeFromDigits(self, A):
        """
        :type A: List[int]
        :rtype: str
        """
        ans=-1
        for h1,h2,m1,m2 in itertools.permutations(A):
            hours=h1*10+h2
            mins=m1*10+m2
            time=hours*60+mins
            if 0<=hours<24 and 0<=mins<60 and time>ans:
                ans=time
            return "{:02}:{:02}".format(divmod(ans,60)) if ans>=0 else ""
```

```C++
//C++部分需要使用STL库里的<algorithm>中的next_permutation函数,详情可以参照：
//http://www.cnblogs.com/eudiwffe/p/6260699.html

class Solution {
public:
    string largestTimeFromDigits(vector<int>& A) {
        //先对A进行排序
        sort(A.begin(),A.end());
        int ans=-1;
        do{
            ans=max(ans,isValid(A));

        }while(next_permutations(A.begin,A.end()));
        if(ans<0）return "";
        char buf[10];
        sprintf(buff,"%02d:%02d",t/60,t%60);
        return buf;

    }

    int isValid(vecotr<int> &A){
        int hours=A[0]*10+A[1];
        int mins=A[2]*10+A[3];
        if(hours<0||hours>=24) return -1;
        if(mins<0||mins>=60) return -1;
        return hours*60+mins;
    }

};
```


#### [951. Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees/)

题目大意：我们可以为二叉树 T 定义一个翻转操作，如下所示：选择任意节点，然后交换它的左子树和右子树。
只要经过一定次数的翻转操作后，能使 X 等于 Y，我们就称二叉树 X 翻转等价于二叉树 Y。
编写一个判断两个二叉树是否是翻转等价的函数。这些树由根节点 root1 和 root2 给出。

题目思路：思路比较简单，利用递归。就是只需判断两树根节点是否相同。如不同直接返回false。如果根节点相同，递归判断两树的左子树和右子树是否满足。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def flipEquiv(self, root1, root2):
        """
        :type root1: TreeNode
        :type root2: TreeNode
        :rtype: bool
        """
        if not root1 and not root2:
            return True
        if not root1 or not root2:
            return False
        if root1.val!=root2.val:
            return False
        //左子树与左子树比较
        bool1=self.flipEquiv(root1.left,root2.left) and self.flipEquiv(root1.right,root2.right) 
        //左子树和右子树比较
        bool2=self.flipEquiv(root1.left,root2.right) and self.flipEquiv(root1.right,root2.left)
        return bool1 or bool2
```