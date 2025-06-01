# week 13
### 316_Remove Duplicate Letters
資料結構：Stack, Set, Hash Map
演算法：Greedy
#### Code
```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        
        stack = [] 
        # 紀錄目前已經放進stack的字母
        seen = set()
        # 記錄每個字母最後一次出現的位置
        last_occurr = {ch: i for i, ch in enumerate(s)}

        for i, ch in enumerate(s):
            if ch in seen:
                continue

            # 若當前字母比stack小，且stack之後還會出現->可pop掉
            while stack and ch < stack[-1] and i < last_occurr[stack[-1]]:
                removed = stack.pop()
                seen.remove(removed)

            stack.append(ch)
            seen.add(ch)

        return ''.join(stack)
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/982f63ea-5f1e-4ded-8e94-2657cfd317b0)

