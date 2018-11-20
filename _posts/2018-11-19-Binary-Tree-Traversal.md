---
layout: post
title:  "Binary Tree Traversal"
categories: LeetCode
tags: Tree DataStrcuture 
author: ccpocker
---

* content
{:toc}


## 1.综述

二叉树的遍历主要分为两大类：
* 层次遍历
* 非层次遍历

其中非层次遍历中包含下面三种：
* 前序遍历
* 中序遍历
* 后序遍历

非层次遍历使用递归较为简单，如需使用循环，则需要用到栈、队列等数据结构。

## 2.层次遍历
二叉树的层次遍历，实际上就是广度优先遍历（BFS).按照树的层次从高到低，每层依次从左至右，访问节点。

层次遍历对应Leetcode.102题:

[102. Binary Tree Level Order Traversal]("https://leetcode.com/problems/binary-tree-level-order-traversal/description/")

### C++:

```Cpp
方法一:递归方法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
 class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        Build(root,0,res);
        return res;
    }
    void Build(TreeNode* node,int level,vector<vector<int>>& res){
        if(!node) return;
        if(level+1>res.size()){
            res.push_back(vector<int> {});
        }
        res[level].push_back(root->val);
        Build(root->left,level+1,res);
        BUild(root->right,level+1,res);
    }
};



方法二:非递归
```Cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;/**返回的结果**/
        /**创建两个队列，分别用于保存当前层的树节点，和下一层的树节点**/
        queue<TreeNode*> curr,next;
        curr.push(root);
        /**储层每一层节点的值**/
        vector<int> level;
        while(!curr.empty()){
            TreeNode* node=curr.front();
            curr.pop();
            level.push_back(node->val);
            if(node->left) next.push(node->left);
            if(node->right) next.push(node->right);
            /**当前层的节点遍历完毕**/
            if(curr.empty()){
                res.push_back(level);
                curr.swap(next);
                level.clear()
            }
        }
        return res;
    }
};
```

### Python:

## 二叉树的非层次遍历
非层次遍历实际上是深度优先遍历(DFS)，其中三种方式分别为:
* 前序遍历:先访问根节点、再左子树、最后右子树
* 中序遍历:先访问左子树、再根节点、最后右子树
* 后序遍历:先访问左子树、再右子树、最后根节点

对应的LeetCode题目分别是144、94、145。

[144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)、
[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)、
[145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)。

对于非层次遍历,使用递归方法较为简单

``` C++
方法一：递归方法处理非层次遍历
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        helper(root,res);
        return res
    }
    void helper(TreeNode* node,vecotr<int>& res){
        if(!node) return;
        /**先序遍历**/
        res.push_back(node->val);
        helper(node->left,res);
        helper(node->right,res);
        
        /**中序遍历**/
        // helper(node->left,res);
        // res.push_back(node->val);
        // helper(node->right,res);
        
        /**后序遍历**/
        // helper(node->left,res);
        // helper(node->right,res);
        // res.push_back(node->val,res)
        
    }
};

方法二：非递归方法处理

class Solution {
public:
    //前序遍历
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> s1;
        s1.push(root);
        while(!s1.empty()){
            TreeNode* node=s1.top();
            s1.pop();
            res.push_back(node->val);
            if(node->right) s1.push(node->right);
            if(node->left)  s1.push(node->left);
        }
        return res;
    }
    //中序遍历
    vector<int> inorderTraversal(TreeNode* root){
        vector<int> res;
        stack<TreeNode*> s1;
        TreeNode* pCurrent=root;
        while(!s1.empty()||pCurrent){
            if(pCurrent){
                stack.push(pCurrent);
                pCurrent=pCurrent.left;
            }
            else{
                TreeNode* node=s1.top();
                res.push_back(node->val);
                s1.pop()
                pCurrent=node->right;
            }
        }
        return res;
    }

    //后序遍历
    //注意到后序遍历反转是根节点-右子树-左子树，修改前序遍历即可
    vector<int> postorderTraversal(TreeNode* root) {
        //使用deque、可省略最后reverse的过程
        deque<int> res;
        stack<TreeNode*> s1;
        s1.push(root);
        while(!s1.empty()){
            TreeNode* node=s1.top();
            s1.pop();
            //进队列头
            res.push_front(node->val);
            if(node->left) s1.pop(node->left);
            if(node->right) s1.pop(node->right);
        }
        return vector<int>(ans.begin(),ans.end())
    }
};
```

