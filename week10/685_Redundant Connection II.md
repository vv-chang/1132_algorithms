# week 10
### 685_Redundant Connection II
資料結構：Graph,dict<br>
演算法：DFS
#### Code
```python
class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        from collections import defaultdict

        n = len(edges)
        parent = {}
        cand1 = cand2 = None

        for u, v in edges:
            if v in parent:
                cand1 = [parent[v], v]
                cand2 = [u, v]
                break
            parent[v] = u

        def has_cycle(graph):
            visited = [0] * (n + 1)

            def dfs(node):
                if visited[node] == 1:
                    return True
                if visited[node] == 2:
                    return False
                visited[node] = 1
                for nei in graph[node]:
                    if dfs(nei):
                        return True
                visited[node] = 2
                return False

            for i in range(1, n + 1):
                if visited[i] == 0:
                    if dfs(i):
                        return True
            return False

        def build_graph(skip_edge):
            graph = defaultdict(list)
            for u, v in edges:
                if [u, v] == skip_edge:
                    continue
                graph[u].append(v)
            return graph

        if cand1:
            graph = build_graph(cand2)
            if not has_cycle(graph):
                return cand2 
            else:
                return cand1 

        graph = build_graph(None)
        for i in range(len(edges) - 1, -1, -1):
            u, v = edges[i]
            graph[u].remove(v)
            if not has_cycle(graph):
                return [u, v]
            graph[u].append(v)
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/3aa340ea-25f3-4279-b40f-da759782ffec)
