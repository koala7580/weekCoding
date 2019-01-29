# 二叉树的遍历  
二叉树的递归遍历和非递归遍历，以及层次遍历。另外一个栈的后序遍历非递归实现、Mirror遍历没有温习，附有链接，后期补充。  
### code  

```python
class TreeNode():
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

# 创建二叉树
def create_tree(root):
    element = input("Enter a key:")
    if element == '#':
        root = None
    else :
        root = TreeNode(element)
        root.left = create_tree(root.left)
        root.right = create_tree(root.right)
    return root

# --------------------------------------
# 递归遍历
# --------------------------------------

# 先序遍历
def pre_order(root):
    if root:
        print(root.val, end='')
        pre_order(root.left)
        pre_order(root.right)

# 中序遍历
def mid_order(root):
    if root:
        mid_order(root.left)
        print(root.val, end='')
        mid_order(root.right)

# 后序遍历
def post_order(root):
    if root:
        post_order(root.left)
        post_order(root.right)
        print(root.val, end='')

# --------------------------------------
# 非递归遍历
# --------------------------------------

# 非递归 先序遍历
def preorder(root):
    if not root:
        return
    stack = []      # 先序遍历，先入后出，根-左-右
    while root or len(stack):
        if root:
            stack.append(root)
            print(root.val, end='')
            root = root.left
        else :
            root = stack.pop()         
            root = root.right

# 非递归 中序遍历
def midorder(root):
    if not root:
        return
    stack = []      # 中序遍历，左子树-根-右子树
    while root and len(stack):
        if root:
            stack.append(root)
            root = root.left
        else:
            print(root.val, end='')
            root = stack.pop()
            root = root.right

# 非递归 后序遍历
def postorder(root):
    if not root:
        return
    stack = []          # 节点添加顺序：根-右-左
    stack_val = []      # 节点访问的逆序：左-右-根
    while root and len(stack):
        if root:
            stack.append(root)
            stack_val.append(root.val)
        else:
            root = stack.pop()
            root = root.left
    while stack_val:
        print(stack_val.pop(), end='')
 
# --------------------------------------
# 递归遍历
# --------------------------------------

# 层次遍历
def levelorder(root):
    if not root:
        return
    queue = []      # 队列，先入先出
    queue.append(root)
    while queue:
        root = queue.pop(0)
        print(root.val, end='')
        if root.left:
            queue.append(root.left)
        if root.right:
            queue.append(root.right)

```

还有一个栈的后序遍历和Morris遍历，今天事情有点多，先不复习了，后期补。之前做题时一直跟着下面这个大佬，但是没能坚持下来，现在继续。  
推荐链接：  

[剑指offer的python实现](https://blog.csdn.net/qq_34342154/article/details/76131445)  
[Morris](https://www.cnblogs.com/AnnieKim/archive/2013/06/15/morristraversal.html)  

# problem  98. Validate Binary Search Tree  
## describle 
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

 - The left subtree of a node contains only nodes with keys less than the node's key.
 - The right subtree of a node contains only nodes with keys greater than the node's key.
 - Both the left and right subtrees must also be binary search trees.   
   
**Example 1:**
```
Input:
    2
   / \
  1   3
Output: true
```  

**Example 2:**
```
Input:
    5
   / \
  1   4
     / \
    3   6
Output: false

Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value
             is 5 but its right child's value is 4.  
```    

*题意*  
判断给定的数是不是合法的BST。即当前节点值比他左子树大，比右子树小。

## method  
中序遍历二叉树，负无穷 < 左子树 < 根 < 右子树 < 正无穷  
### code  
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        
        return self.isValid(root, -float('inf'), float('inf'))
        
    def isValid(self, root, min, max):
        if not root :
            return True
        if root.val <= min or root.val >=  max:
            return False

        return self.isValid(root.left, min, root.val) and self.isValid(root.right, root.val, max)
```

# problem   102. Binary Tree Level Order   
## describle  
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree ```[3,9,20,null,null,15,7]```,  
```
    3
   / \
  9  20
    /  \
   15   7
```  
return its level order traversal as:  
```  
[
  [3],
  [9,20],
  [15,7]
]  
```  
*题意*  
层次遍历二叉树，返回每层list结构  
## method   
先序遍历二叉树，用level记录每层的高度，root 的 level=0。  
### code  
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """        
        result = []
        self.pre_order(root, 0, result)
        return result
    
    def pre_order(self, root, level, result):
        if not root:
            return
        if len(result) < level + 1:
            result.append([])
        result[level].append(root.val)
        self.pre_order(root.left, level + 1, result)
        self.pre_order(root.right, level + 1, result)
```

# problem   107. Binary Tree Level Order Traversal II  
## describle  
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree ```[3,9,20,null,null,15,7]```,    
```
    3
   / \
  9  20
    /  \
   15   7
```  
return its bottom-up level order traversal as:  
```
[
  [15,7],
  [9,20],
  [3]
]
```  
*题意*  
层次遍历二叉树，从底向上的返回每层list结构。 
## method  
先序遍历二叉树，用level记录每层的高度，root 的 level=0,结果记录时，从列表开始位置插入，level与索引对应关系为：  
``` index = -(level+1) ```  
### code    

*python*  
```python 
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        result = []
        self.pre_order(root, 0, result)
        return result
    
    def pre_order(self, root, level, result):
        if not root:
            return
        if len(result)<level+1:
            result.insert(0, [])
        result[-(level+1)].append(root.val)
        self.pre_order(root.left, level+1, result)
        self.pre_order(root.right, level+1, result)
```

*附以前的c++版本*  
非递归的方法，层次记录，结果逆序。以前用vector觉得超级好用，不过好久不用都忘记了……  

```cpp
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> level,levelBU;
        vector<int> level_each;
        if(!root)
            return level;
        queue<TreeNode* > que;
        que.push(root);
        
        while(!que.empty()){
            int n=que.size();
            TreeNode* tmp;
            level_each.clear();
            for(int i=0;i<n;i++){
                tmp=que.front();
                level_each.push_back(tmp->val);
                que.pop();
                if(tmp->left) que.push(tmp->left);
                if(tmp->right) que.push(tmp->right);
            }
            level.push_back(level_each);
        }
        for(int i=level.size()-1;i>=0;i--){
            levelBU.push_back(level[i]);
        }
        return levelBU;
    }
};
```

二叉树这块主要是递归的思想，递归用的好的话代码会很简洁明了，但是性能要比非递归的差一点。但是自己对这块掌握的不太好，一写起递归或者循环嵌套的代码就容易混乱。还需加强练习，后续有时间再温习二叉树相关知识。

*参考链接*  
[98. Validate Binary Search Tree](https://www.cnblogs.com/slurm/p/5221590.html)  
[102 Binary Tree Level Order Traversal](https://www.cnblogs.com/loadofleaf/p/5502285.html)  
