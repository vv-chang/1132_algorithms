# week 1
### 460_LFU_Cache
LFU Cache (Least Frequently Used)：容量達到上限時，移除使用頻率最低的數據
#### Code
```python
from collections import defaultdict, OrderedDict

class LFUCache:
    def __init__(self, capacity: int):
        
        self.capacity = capacity
        
        # 存 key對應 value
        self.key_to_val_freq = {}  # 存 key -> (value, freq) {} -->dict
        self.freq_to_keys = defaultdict(OrderedDict)  # 存 freq -> {key: None}，OrderedDict 保持順序 # {{1,3}{2,5}{4,6}} --> O(1) 陣列做 pop 是O(N)
        self.min_freq = 0  # 紀錄當前最小頻率

    def _update_freq(self, key: int):

        ## (1) key_to_val_freq # Dict
        # {key1:(value,freq), key2:(value,freq), key3:(value,freq)}
        ## (2) freq_to_keys # OrderedDict
        # {3:{key1:none, key2:none, key3:none}, 4:{key4:none}}
        # 3--> freq ; 是 dict 的 key (非 key1, key2...etc) 此 key 非彼 key
        
        # 更新 key 的使用頻率
        value, freq = self.key_to_val_freq[key] # pair 
        
        del self.freq_to_keys[freq][key]  # 刪除舊頻率的 key
        if not self.freq_to_keys[freq]:  # 如果舊頻率沒有 key
            del self.freq_to_keys[freq]
            
            if self.min_freq == freq:
                self.min_freq += 1  # 若刪除的是 min_freq，則遞增

        # 更新 key 的頻率
        self.key_to_val_freq[key] = (value, freq + 1)
        self.freq_to_keys[freq + 1][key] = None  # 插入到新的頻率

    def get(self, key: int) -> int:
        
        if key not in self.key_to_val_freq:
            return -1
        self._update_freq(key)  # 更新頻率
        return self.key_to_val_freq[key][0]  # [0] --> value

    def put(self, key: int, value: int) -> None:
        
        if self.capacity == 0:
            return

        if key in self.key_to_val_freq:
            self.key_to_val_freq[key] = (value, self.key_to_val_freq[key][1])  # 更新值 pair(value, self.key_to_val_freq[key][1]=原本的freq)
            self._update_freq(key)  # 更新頻率
            return

        if len(self.key_to_val_freq) >= self.capacity:
            # 移除最不常用的 key（在 min_freq 下的最舊 key）
            old_key, _ = self.freq_to_keys[self.min_freq].popitem(last=False) # 僅要調用 old_key，但一次會調用一組pair
            del self.key_to_val_freq[old_key]

        # 如果前二種情況就直接 return 不會執行到這邊
        # 插入新的key，頻率為 1
        self.key_to_val_freq[key] = (value, 1)
        self.freq_to_keys[1][key] = None
        self.min_freq = 1  # 更新最小頻率
```
#### Accepted Pic
![w1_leedcode460](https://github.com/user-attachments/assets/b6891f7b-b99e-4042-a920-31032dbb8b84)
