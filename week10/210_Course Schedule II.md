# week 10
資料結構：Graph,queue<br>
演算法：BFS
### 210_Course Schedule II
#### Code
```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        
        # 建表
        graph = [[] for _ in range(numCourses)]
        in_degree = [0] * numCourses

        for pair in prerequisites:
            course, pre = pair
            graph[pre].append(course)
            in_degree[course] += 1
            
        queue = deque()
        for i in range(numCourses):
            if in_degree[i] == 0:
                queue.append(i)

        result = []

        while queue:
            current = queue.popleft()
            result.append(current)
            for neighbor in graph[current]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)

        # 判斷是否所有課程都已加入 result
        if len(result) == numCourses:
            return result
        else:
            return []
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/4aec5fea-e785-4835-b22c-13583b3fb545)
