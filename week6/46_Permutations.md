
# week 6
### 46_Permutations
#### Code
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []

        def backtrack(path, remaining):
            # 若沒有剩下的數字可以選，代表已經完成一組排列
            if not remaining:
                res.append(path[:])  # 將當前排列加入結果
                return

            # 遍歷所有剩下的數字
            for i in range(len(remaining)):
                # 選擇第 i 個數字，加入當前 path
                # 然後從剩下的數字中排除這個數字，繼續遞迴
                backtrack(path + [remaining[i]], remaining[:i] + remaining[i+1:])

        # 從空的開始遞迴
        backtrack([], nums)
        return res
```
#### Accepted Pic
![螢幕擷取畫面 2025-04-07 232010](https://github.com/user-attachments/assets/06756e83-51be-4b83-89a5-4f8d0eb13185)
