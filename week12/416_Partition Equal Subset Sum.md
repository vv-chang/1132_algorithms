# week 12
### 416_Partition Equal Subset Sum
演算法：dp
#### Code
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        if total % 2 != 0:
            return False
        
        target = total // 2
        dp = [False] * (target + 1)
        dp[0] = True

        for num in nums:
            for i in range(target, num-1, -1):
                dp[i] = dp[i] or dp[i - num]
        
        return dp[target]
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/d1bbecb0-1c12-45cf-94de-dee95f00dc0a)
