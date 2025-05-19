# week 12
### 312_Burst Balloons
演算法：dp
#### Code
```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        n = len(nums)
        dp = [[0] * n for _ in range(n)]

        for length in range(2, n):
            for left in range(0, n - length):
                right = left + length
                for i in range(left + 1, right):
                    dp[left][right] = max(dp[left][right],
                                          dp[left][i] + nums[left] * nums[i] * nums[right] + dp[i][right])
        return dp[0][n - 1]
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/6a5e7def-2a8c-49c5-b56c-bb073147473b)
