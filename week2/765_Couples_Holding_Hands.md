# week 2
### 765_Couples_Holding_Hands
#### Code
```python
from typing import List

class Solution:
    def minSwapsCouples(self, row: List[int]) -> int:
        n = len(row)//2
        sit = {}
        change = 0

        for i in range(len(row)):
            # 記錄誰坐在哪
            person = row[i]
            sit[person] = i

        # 情侶坐在一起
        for i in range(0, len(row), 2):
            first = row[i]
            
            if first % 2 == 0:
                couple = first+1 # 偶數
            else:
                couple = first-1  # 奇數

            # 已坐在一起
            if row[i + 1] == couple:
                continue  
            couple_sit = sit[couple] 

            row[i + 1], row[couple_sit] = row[couple_sit], row[i + 1]

            sit[row[couple_sit]] = couple_sit
            sit[row[i + 1]] = i + 1
            change += 1  

        return change
```
#### Accepted Pic
![w2_leetcode765](https://github.com/user-attachments/assets/66782f41-a601-4596-b8b5-820b1d196074)

