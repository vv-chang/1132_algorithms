# week 5
### 105_Construct_Binary_Tree_from_Preorder_and_Inorder_Traversal
使用 Binary Tree + Hash Map, Divide and Conquer + Recursion
#### Code
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        # 建立一個值到index的mapping，加速查找
        idx_map = {val: i for i, val in enumerate(inorder)}
        self.pre_idx = 0  # 用來追蹤目前在 preorder 的位置

        # 定義遞迴函數，建構子樹
        def helper(in_left: int, in_right: int) -> Optional[TreeNode]:
            # 若範圍錯代表無節點
            if in_left > in_right:
                return None

            # 從preorder中取得root的值
            root_val = preorder[self.pre_idx]
            self.pre_idx += 1

            # 建立root節點
            root = TreeNode(root_val)

            # 取得該root在inorder中的位置
            index = idx_map[root_val]

            # 遞迴建左、右子樹（照inorder的分割）
            root.left = helper(in_left, index - 1)
            root.right = helper(index + 1, in_right)

            return root

        # 初始情況：整個 inorder 的範圍
        return helper(0, len(inorder) - 1)
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/6e633d95-698c-41cb-8520-7936d341605e)



