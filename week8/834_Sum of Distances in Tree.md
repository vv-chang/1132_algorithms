# week 8
### 834_Sum of Distances in Tree
#### Code
```python
class Solution:
    def sumOfDistancesInTree(self, n: int, edges: List[List[int]]) -> List[int]:
        tree = defaultdict(list)
        for u, v in edges:
            tree[u].append(v)
            tree[v].append(u)
        
        res = [0] * n    # 最後答案，每個點到所有點的總距離
        count = [1] * n  # count[i] = 以 i 為根的子樹大小（包括自己）

        # 第一次 DFS：從 root (0) 出發，計算 res[0]（root 到所有點的總距離），並順便記錄 count[]
        def dfs(node, parent):
            for nei in tree[node]:
                if nei != parent:
                    dfs(nei, node)
                    count[node] += count[nei]
                    res[node] += res[nei] + count[nei]
        
        # 第二次 DFS：根據 parent 的結果推導 child 的結果
        def dfs2(node, parent):
            for nei in tree[node]:
                if nei != parent:
                    # 公式：res[child] = res[parent] - count[child] + (n - count[child])
                    res[nei] = res[node] - count[nei] + (n - count[nei])
                    dfs2(nei, node)
        
        dfs(0, -1)
        dfs2(0, -1)
        return res
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/f189bb2d-9e60-4c77-9eb9-3ff2f97cc83c)
