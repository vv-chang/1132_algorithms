# week 14
### 347_Top K Frequent Elements
資料結構：Hash Map, Heap
#### Code
```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        # 統計每個數的出現次數
        freq = Counter(nums)
        # 使用max heap
        return [item for item, _ in heapq.nlargest(k, freq.items(), key=lambda x: x[1])]
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/53c9d0d6-2c96-42e2-9329-91c0b2fb150f)

