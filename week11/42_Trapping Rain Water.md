# week 11
### 42_Trapping Rain Water
演算法：two pointer
#### Code
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height:
            return 0

        left, right = 0, len(height) - 1  
        left_max, right_max = 0, 0
        ans = 0

        while left < right:
            if height[left] < height[right]:
                # 左邊較低: 處理左邊
                if height[left] >= left_max:
                    left_max = height[left] 
                else:
                    ans += left_max - height[left] 
                left += 1
            else:
                # 右邊較低：處理右邊
                if height[right] >= right_max:
                    right_max = height[right]
                else:
                    ans += right_max - height[right]
                right -= 1

        return ans
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/ae66c7a5-02b6-44a1-a590-89c6a89be104)
