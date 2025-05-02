# week 9
### 2742_Painting the Walls
#### Code
```python
class Solution:
    def paintWalls(self, cost: List[int], time: List[int]) -> int:
        n = len(cost)
        INF = float('inf')
        
        # dp[i]：完成 i 面牆的最小花費
        dp = [INF] * (n + 1)
        dp[0] = 0  # 沒有牆要漆時，不需花錢

        for i in range(n):
            c, t = cost[i], time[i]
            # 倒著走，避免重複更新同一個牆數
            for j in range(n, -1, -1):
                if dp[j] == INF:
                    continue
                # 請這個付費工人後，可以完成 (1 + time[i]) 面牆
                new_j = min(n, j + 1 + t)
                dp[new_j] = min(dp[new_j], dp[j] + c)

        return dp[n]
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/036697ff-20db-434d-8c9b-c26f7eac87ce)

