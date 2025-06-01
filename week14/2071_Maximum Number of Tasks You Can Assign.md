# week 14
### 2071_Maximum Number of Tasks You Can Assign
資料結構：Deque 演算法：Binary Search, Greedy
#### Code
```python
class Solution:
    def _check(self, tasks: List[int], workers: List[int], pills: int, strength: int) -> bool:
        boosted = deque()
        w = len(workers) - 1

        for t in reversed(tasks):
            if boosted and boosted[0] >= t:
                boosted.popleft()
            elif w >= 0 and workers[w] >= t:
                w -= 1
            else:
                while w >= 0 and workers[w] + strength >= t:
                    boosted.append(workers[w])
                    w -= 1
                if not boosted or pills == 0:
                    return False
                boosted.pop()
                pills -= 1
        return True

    def maxTaskAssign(self, tasks: List[int], workers: List[int], pills: int, strength: int) -> int:
        tasks.sort()
        workers.sort()

        i, j = 0, min(len(tasks), len(workers))

        while i < j:
            mid = (i + j + 1) // 2
            if self._check(tasks[:mid], workers[-mid:], pills, strength):
                i = mid
            else:
                j = mid - 1

        return i
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/f1ec70bb-03d7-4c7f-aaf4-1fcda797f097)

