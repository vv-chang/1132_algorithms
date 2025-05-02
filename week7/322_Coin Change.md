# week 7
### 322_Coin Change
#### Code
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        # 如果目標金額是 0，直接回傳 0
        if amount == 0:
            return 0
        
        # 建立一個 queue，裡面放 (目前金額, 使用的硬幣數量)
        queue = deque()
        queue.append((0, 0))  # (目前總金額, 硬幣數量)
        
        # 用 set 紀錄已經訪問過的金額，避免重複計算
        visited = set()
        visited.add(0)
        
        while queue:
            current_amount, steps = queue.popleft()
            
            # 嘗試每一種硬幣
            for coin in coins:
                next_amount = current_amount + coin
                # 如果剛好湊到 amount，回傳目前步數+1
                if next_amount == amount:
                    return steps + 1
                # 如果還沒到且沒超過，且這個金額沒看過，就加入 queue
                if next_amount < amount and next_amount not in visited:
                    visited.add(next_amount)
                    queue.append((next_amount, steps + 1))
        
        # 如果無法湊到目標金額
        return -1
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/59cc2b49-b865-4b41-b325-a7214645f313)

