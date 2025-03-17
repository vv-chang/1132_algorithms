# week 2
### 45_Jump_Game_II
#### Code
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        if n == 1:
            return 0
    
        jump = 0
        end = 0
        far = 0
    
        for i in range(0, n-1, 1):

            # 更新最遠能到的位置
            far = max(far, i+nums[i])  

            # 範圍達邊界，跳躍+1
            if i == end:  
                jump += 1
                end = far

                # 抵達終點
                if end >= n-1:  
                    return jump
    
        return jump
```
#### Accepted Pic
![w2_leetcode45](https://github.com/user-attachments/assets/ffc313a2-baa5-41ef-a341-378db3c7c1e8)
