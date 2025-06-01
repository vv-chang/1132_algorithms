# week 13
### 895_Maximum Frequency Stack
資料結構：Stack, Hash Map
演算法：Greedy
#### Code
```python
class FreqStack:

    def __init__(self):
        self.freq = defaultdict(int)
        self.group = defaultdict(list)
        self.maxFreq = 0

    def push(self, val: int) -> None:
        f = self.freq[val] + 1
        self.freq[val] = f
        self.group[f].append(val)
        if f > self.maxFreq:
            self.maxFreq = f

    def pop(self) -> int:
        val = self.group[self.maxFreq].pop()
        self.freq[val] -= 1
        if not self.group[self.maxFreq]:
            self.maxFreq -= 1
        return val
        
# Your FreqStack object will be instantiated and called as such:
# obj = FreqStack()
# obj.push(val)
# param_2 = obj.pop()
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/37ea0c64-767d-4f33-88eb-55febd387215)
