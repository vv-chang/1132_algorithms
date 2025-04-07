# week 6
### 46_Permutations
資料結構：List + Set<br>
演算法：Backtracking
#### Code
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        def backtrack(row: int):
            # 如果已經放完所有行-->找到一組解
            if row == n:
                board = []
                # 將queens陣列轉換成字串表示的棋盤
                for i in range(n):
                    row_str = ['.'] * n   # 初始化一行為全-->.
                    row_str[queens[i]] = 'Q'  # 在對應的位置放皇后
                    board.append("".join(row_str))  # 加入棋盤
                res.append(board)
                return
            
            # 嘗試在當前 row 的每一列放置皇后
            for col in range(n):
                # 檢查是否安全
                if col in cols or (row - col) in diag1 or (row + col) in diag2:
                    continue  # 不安全-->跳過

                # 記錄皇后的位置
                queens[row] = col
                cols.add(col)            # 標記已使用
                diag1.add(row - col)     # 標記左上到右下對角線已使用
                diag2.add(row + col)     # 標記右上到左下對角線已使用

                # 遞迴
                backtrack(row + 1)

                # 回溯（撤銷這步，回到前一動）
                cols.remove(col)
                diag1.remove(row - col)
                diag2.remove(row + col)

        res = [] 
        queens = [-1] * n  # queens[i] 代表第 i 行的皇后放在第幾列
        cols = set()   # 記錄已放皇后的欄位
        diag1 = set()  # 對角線（row-col）
        diag2 = set()  # 反對角線（row+col）

        backtrack(0)  # 從第0行開始遞迴嘗試
        return res
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/f7077f1e-a010-415b-b5cd-e47770c31817)

