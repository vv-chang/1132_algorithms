
```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
    
        result = []
        dq = deque()
        
        if not nums:
            return []
        
        for i in range(0, len(nums), 1):
            if dq and dq[0] < i - k + 1:
                dq.popleft()

            while dq and nums[dq[-1]] < nums[i]:
                dq.pop()
            
            dq.append(i)
            
            # 紀錄當前Max
            if i >= k - 1:
                result.append(nums[dq[0]])
        
        return result
```
