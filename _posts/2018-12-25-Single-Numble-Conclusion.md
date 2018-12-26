---
layout: post
title:  LeetCode Single Number Conclusion
categories: LeetCode
tags: 每日一题  Bit-Manipulation
author: ccpocker
---

* content
{:toc}


### Single Number Conclusion
Single Number问题主要是针对bit运算的一类问题。分别有三种类型，本次主要针对三个leetcode对应的题目，进行解答，并对bit-Manipulation进行相关整理和总结。

#### [136. Single Number](https://leetcode.com/problems/single-number/)
题目描述：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素

这道题目有两种解法：一种使用Hash表，由于hash表的查找效率是O(1),所以时间复杂度是O(n)，空间复杂度也是O(n).

这道题要求空间复杂度是O(1),使用bit-Manipulation，具体而言使用位运算的异或。这是因为a^a=0,a^0=a。所以对于出现两次的整数进行异或运算都会变成0.最后只剩下出现一次的数。
```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res=0;
        for(const int&num:nums)
            res^=num;
        return res;
    }
};
```


####[137. Single Number II](https://leetcode.com/problems/single-number-ii/)
题目描述：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现三次。找出那个只出现了一次的元素。

对于这个题目，只是从上题出现两次变成出现三次。所以需要挖掘内在的逻辑关系。

解法一：
我们可以建立一个32位的数字，来统计每一位1出现的次数，如果某一位为1的话，那么如果该整数出现了三次，对3取余为0，我们把每个数的对应位都加起来对3取余，最后剩下来的那个数就是单独的数字。
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res=0;
        for(int i=0;i<32;i++){
            int sum=0;
            for(int j=0;j<nums.size();j++){
                sum+=(nums[j]>>i)&1;
            }
            res|=(sum%3)<<i;
        }
        return res;
    }
};
```
解法二：用三个整数表示INT的各位出现的情况，one表示出现一次，two表示出现两次。出现三次时，该位清0。所以最后的答案时one的值。

假设现在有一个数字1，那么one的更新方式就是**异或**这个数字1(0\^1=0,1^1=0).而two的更新方式是用上一个状态下的one**与**这个数字（1&1=1,1&0=0).注意更新顺序是先更新two，再更新one。然后更新three，由于之前已经更新了two然后更新了one，如果此时two=1，one也为1.那么此时这个数字出现了三次，需要将one和two置0，所以one和two**与**上three的相反数。

class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int one=0,two=0,three=0;
        for(int&num:nums){
            two|=one&num;
            one=one^num;
            three=one&two;
            one&=~three;
            two&=~three;
        }
        return one;
    }
};
