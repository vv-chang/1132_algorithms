# week 2
### 2366_Minimum_Replacements_to_Sort_the_Array
#### Code
```python
from typing import List

class Solution:
    def minimumReplacement(self, nums: List[int]) -> int:
        n = len(nums)
        less = 0

        ini = nums[-1] #最後一個數

        # 反向遍歷，右到-->非遞減順序
        for i in range(n - 2, -1, -1):
            if nums[i] > ini:
                k = (nums[i] + ini -1) // ini  
              
                less += k-1  
                ini = nums[i]// k  
            else:
                # 若小於等於 ini不用分割，直接更新 ini
                ini = nums[i]  
        
        return less
```

### 765. Couples Holding Hands
```

```
#### Accepted Pic![w2_leetcode2366](https://github.com/user-attachments/assets/6076cd17-0b31-4984-8c01-cd1007cad5b0)

