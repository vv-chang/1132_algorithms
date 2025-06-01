# week 14
### 220_Contains Duplicate III
資料結構：Hash Map 演算法：Bucket Sort, Sliding Window
#### Code
```python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], indexDiff: int, valueDiff: int) -> bool:
        if valueDiff < 0:
            return False

        bucket = {}
        width = valueDiff + 1

        for i, num in enumerate(nums):
            bucket_id = num // width

            # 檢查相同桶
            if bucket_id in bucket:
                return True

            # 檢查左邊桶
            if bucket_id - 1 in bucket and abs(num - bucket[bucket_id - 1]) <= valueDiff:
                return True

            # 檢查右邊桶
            if bucket_id + 1 in bucket and abs(num - bucket[bucket_id + 1]) <= valueDiff:
                return True

            bucket[bucket_id] = num

            if i >= indexDiff:
                old_bucket_id = nums[i - indexDiff] // width
                del bucket[old_bucket_id]

        return False
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/f4718d1e-5260-4bd3-8c81-58726fcec657)

