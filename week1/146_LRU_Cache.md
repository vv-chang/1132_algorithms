# week 1
## 146_LRU_Cache
### Code
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:

        if key not in self.cache:
            return -1
            
        # 調用這個key，移到cache最後面
        self.cache.move_to_end(key, last=True)

        # 回傳這個key的value
        return self.cache[key]

    def put(self, key:int, value:int) -> None:
        
        if key in self.cache:
            self.cache.move_to_end(key, last=True)

        self.cache[key] = value

        if len(self.cache) > self.capacity:
            # 把第一個丟出去
            self.cache.popitem(last=False)
```
### Accepted Pic
![w1_leedcode146](https://github.com/user-attachments/assets/01295c5c-cd6e-4c4d-975d-d6f232af1aee)
