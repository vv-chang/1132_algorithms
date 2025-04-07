# week 5
### 912_Sort_an_Array
演算法：Divide and Conquer + Recursion + Merge sort
#### Code
```python
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        def merge_sort(arr: List[int]) -> List[int]:

            # 當陣列長度為 0 或 1 時，直接回傳（已排序好）
            if len(arr) <= 1:
                return arr

            # Divide：將陣列從中間切成左右兩半
            mid = len(arr) // 2
            left = merge_sort(arr[:mid])    # 遞迴排序左半部
            right = merge_sort(arr[mid:])   # 遞迴排序右半部

            # Conquer：合併左右已排序的子陣列
            return merge(left, right)

        def merge(left: List[int], right: List[int]) -> List[int]:
            
            # 建立一個空的結果陣列
            result = []
            i = j = 0

            # 同時走訪左右兩個子陣列，進行比較並加入較小的元素
            while i < len(left) and j < len(right):

                if left[i] < right[j]:
                    result.append(left[i])  # left的元素較小，加入
                    i += 1
                else:
                    result.append(right[j])  # right的元素較小，加入
                    j += 1

            # 若 left 陣列有剩下的元素，加入
            result.extend(left[i:])
            # 若 right 陣列有剩下的元素，加入
            result.extend(right[j:])
            return result


        # 呼叫 merge_sort 排序整個 nums
        return merge_sort(nums)
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/b701969b-0cda-477c-9d11-54f743850e47)




