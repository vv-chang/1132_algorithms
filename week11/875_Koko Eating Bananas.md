# week 11
### 875_Koko Eating Bananas
演算法：Binary Search Greedy
#### Code
```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        # 搜尋範圍：min: 1; max：max(piles)
        left, right = 1, max(piles)
        
        def can_finish(speed: int) -> bool:
            # 是否能在h小時內吃完所有香蕉
            total_hours = 0
            for pile in piles:
                # 每堆香蕉吃完所需時間
                total_hours += math.ceil(pile / speed)
            return total_hours <= h
        
        # Binary Search
        while left < right:
            mid = (left + right) // 2
            if can_finish(mid):
                right = mid
            else:
                left = mid + 1
            
        return left
 ```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/9503bf11-8b3c-46fd-941d-fb6672d0468f)
