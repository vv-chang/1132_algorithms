# week 10
### 847_Shortest Path Visiting All Nodes
資料結構：Graph,queue<br>
演算法：BFS
#### Code
```python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)
        queue = deque()
        visited = set()

        for i in range(n):
            state = (i, 1 << i)
            queue.append((i, 1 << i, 0))
            visited.add(state)

        target = (1 << n) - 1  # 目標是所有node都被拜訪

        while queue:
            node, mask, dist = queue.popleft()
            if mask == target:
                return dist  # 所有node都visit過了，回傳目前步數

            for nei in graph[node]:
                next_mask = mask | (1 << nei)
                state = (nei, next_mask)
                if state not in visited:
                    visited.add(state)
                    queue.append((nei, next_mask, dist + 1))

        return -1
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/8e5a0d5f-5361-46f3-842d-d7b75d6a848b)
