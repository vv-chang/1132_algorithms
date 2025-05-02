# week 9
### 44_Wildcard Matching
#### Code
```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        m, n = len(s), len(p)
        # 建立 dp 表，初始為 False
        dp = [[False] * (n + 1) for _ in range(m + 1)]
        dp[0][0] = True  # 空字串對應空 pattern 是 True

        # 初始化第一列：s 為空字串，p 為多個 '*'
        for j in range(1, n + 1):
            if p[j - 1] == '*':
                dp[0][j] = dp[0][j - 1]
        
        # 開始填表
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if p[j - 1] == '*':
                    # '*' 可以匹配空字元 或 多個字元
                    dp[i][j] = dp[i][j - 1] or dp[i - 1][j]
                elif p[j - 1] == '?' or s[i - 1] == p[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
        
        return dp[m][n]
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/774aab63-4a34-42a1-9d3a-621bba45c04c)

