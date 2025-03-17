# week 3
### 757_Set_Intersection_Size_At_Least
#### Code
#### Accepted Pic
```python
class Solution:
    def intersectionSizeTwo(self, intervals: List[List[int]]) -> int:
        n = []
        righttwo = -1
        rightone = -1
        total = 0
        
        # 每個子陣列的第二個元素，小-->大優先排列
        # x[1]-->最右開始遞增排序；-x[0]-->最左開始遞減排序
        intervals.sort(key=lambda x: (x[1], -x[0]))
        print(intervals)

        # 
        for i in range(0, len(intervals), 1):
            start = intervals[i][0]  # 起點
            end = intervals[i][1]  # 終點
            
            if start > rightone:
                total += 2
                righttwo, rightone = end-1, end

            elif start > righttwo:
                total += 1
                
                righttwo, rightone = rightone, end

        return total
```
![w3_leetcode757](https://github.com/user-attachments/assets/28115f4f-a753-4cf0-9fe8-9cd10b50622f)

