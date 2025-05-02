# week 8
### 124_Binary Tree Maximum Path Sum
#### Code
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.max_sum = float('-inf')  # 初始化最大路徑和為負無限大

        def dfs(node):
            if not node:
                return 0  # 空節點的貢獻值是0

            # 遞迴計算左子樹和右子樹的最大貢獻值
            # 如果子樹的最大貢獻值是負數，代表不取反而更大，所以和0取最大
            left_gain = max(dfs(node.left), 0)
            right_gain = max(dfs(node.right), 0)

            # 以目前這個節點為最高點時，計算通過它的最大路徑和
            current_path_sum = node.val + left_gain + right_gain

            # 更新全局最大路徑和
            self.max_sum = max(self.max_sum, current_path_sum)

            # 回傳從這個節點開始，往下延伸的最大貢獻值（只能選一邊）
            return node.val + max(left_gain, right_gain)

        dfs(root)
        return self.max_sum
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/d8d8f477-0738-44a7-96b5-fcc2c0814608)



