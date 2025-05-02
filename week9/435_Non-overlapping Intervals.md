# week 9
### 435_Non-overlapping Intervals
#### Code
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        
        # 如果沒有任何區間，則不需要刪除
        if not intervals:
            return 0
        
        # 將區間按照「結束時間」由小到大排序
        intervals.sort(key=lambda x: x[1])

        # 記錄目前不重疊區間的結束時間
        end = intervals[0][1]
        count = 0  # 記錄需要移除的區間數量

        # 從第二個區間開始判斷
        for i in range(1, len(intervals)):
            # 如果當前區間的起始時間小於前一個區間的結束時間，表示有重疊
            if intervals[i][0] < end:
                count += 1  # 必須刪除這個區間（保留結束時間早的區間）
            else:
                end = intervals[i][1]  # 更新目前不重疊的結束時間

        return count
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/29d6c693-7d0d-4b70-94b1-2392bb4698e5)

