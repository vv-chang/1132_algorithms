# week 12
### 887_Super Egg Drop
演算法：dp
#### Code
```python
class Solution:
    def superEggDrop(self, k: int, n: int) -> int:

        dp = [0] * (k + 1)
        m = 0
        while dp[k] < n:
            m += 1
            for i in range(k, 0, -1):
                dp[i] = dp[i] + dp[i - 1] + 1
        return m
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/0310df33-3167-434f-8582-d8caa3836a9f)
