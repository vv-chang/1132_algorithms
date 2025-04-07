# week 6
### 140_Word_Break_II
使用DFS+Backtrack+Memoization(DP)
#### Code
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        word_set = set(wordDict)
        memo = {} 

        def dfs(s):
            if s in memo:
                return memo[s]  # 若之前已經處理
            if not s:
                return [""]  # 當s為空字串-->已經成功切完，回傳空字串結尾

            res = []
            for word in word_set:
                if s.startswith(word):
                    remainder = s[len(word):]  # 剩下的子字串（去掉前面已匹配的 word）
                    sub_sentences = dfs(remainder)  # 遞迴處理剩下的子字串

                    # 將當前的 word 與遞迴傳回的子句組合成完整句子
                    for sub in sub_sentences:
                        if sub:
                            res.append(word + " " + sub)  # 中間需要空格
                        else:
                            res.append(word)  # sub為空字串，表示word是最後一個字，不加空格

            memo[s] = res  # 記住這個子字串的所有切法，避免重複計算
            return res

        return dfs(s)  # 從整個字串s做DFS
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/99cf4988-1f97-4a9a-a4c8-86946fc48ec0)

