# week 8
### 236_Lowest Common Ancestor of a Binary Tree
#### Code
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 如果 root 是 None，或者 root 就是 p 或 q，直接回傳 root
        if not root or root == p or root == q:
            return root
        
        # 遞迴往左子樹找
        left = self.lowestCommonAncestor(root.left, p, q)
        # 遞迴往右子樹找
        right = self.lowestCommonAncestor(root.right, p, q)
        
        # 如果左邊和右邊都找到了，代表 p 和 q 分別在左右子樹上，所以 root 就是LCA
        if left and right:
            return root
        
        # 如果只有左邊找到，代表兩個節點都在左邊，回傳左邊
        if left:
            return left
        
        # 如果只有右邊找到，代表兩個節點都在右邊，回傳右邊
        return right
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/c07eaac0-fc4a-436b-807e-944fcceb6840)


