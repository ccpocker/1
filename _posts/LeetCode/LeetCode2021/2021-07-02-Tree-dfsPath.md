---
layout: post
title:  二叉树的路径问题
categories: LeetCode 
tags: Tree
author: ccpocker
---

* content
{:toc}

## 综述
二叉树的路径主要分为两类:
- 一类是自顶向上的:从根结点按照深度优先遍历，从上至下，到某一节点结束。
- 第二类就是非自顶向上的，




#### [LeetCode 257 二叉树所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)


```cpp
class Solution {
public:
    vector<string> res;
    void traversal(TreeNode *node,string path){
        if(node==nullptr) return;
        path+=to_string(node->val);
        if(!node->left&&!node->right){
            res.emplace_back(path);
        }
        if(node->left){
            traversal(node->left,path+"->");
        }
        if(node->right){
            traversal(node->right,path+"->");
        }
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        traversal(root,"");
        return res;
    }
}
```
