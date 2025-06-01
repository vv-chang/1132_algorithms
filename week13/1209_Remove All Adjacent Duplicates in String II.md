# week 13
### 1209_Remove All Adjacent Duplicates in String II
資料結構：Stack
演算法：Greedy
#### Code
```python
class Solution:
    def removeDuplicates(self, s: str, k: int) -> str:
        stack = []  # 放入 (字元,出現次數)

        for char in s:
            if stack and stack[-1][0] == char:
                stack[-1][1] += 1
                if stack[-1][1] == k:
                    stack.pop()  # 刪掉連續k個
            else:
                stack.append([char, 1])

        # 重建字串
        result = ''
        for char, count in stack:
            result += char * count
        return result
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/f9dbfa44-1b42-41f6-9d2f-fd8456dcd0da)
