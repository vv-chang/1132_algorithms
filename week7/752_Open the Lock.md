# week 7
### 752_Open the Lock
#### Code
```python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        dead = set(deadends)
        if "0000" in dead:
            return -1
        if target == "0000":
            return 0
        
        # 正向與反向考量的組合
        begin = set(["0000"])
        end = set([target])
        visited = set(["0000"])
        
        steps = 0
        
        while begin and end:
            # 每次從較小的集合擴展(begin和end輪流)
            if len(begin) > len(end):
                begin, end = end, begin
            
            temp = set()  # 暫存所有考量的可能性
            
            for current in begin:
                if current in dead:
                    continue
                
                for i in range(4):
                    num = int(current[i])
                    for move in [-1, 1]:
                        new_num = (num + move) % 10
                        new_state = current[:i] + str(new_num) + current[i+1:]
                        
                        if new_state in end:
                            return steps + 1  # 兩邊相遇了！
                        
                        if new_state not in visited and new_state not in dead:
                            visited.add(new_state)
                            temp.add(new_state)
            
            begin = temp  # 更新下一層要擴展的節點
            steps += 1
        
        return -1  # 如果找不到
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/6382d443-0254-45a6-860c-0fd60546f830)
