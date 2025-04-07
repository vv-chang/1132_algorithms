# week 5
### 23_Merge_k_Sorted_Lists
資料結構：Linked List<br>
演算法：Divide and Conquer + Recursion + Merge sort
#### Code
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None
        # 合併所有 linked list
        return self.merge_range(lists, 0, len(lists) - 1)
    
    def merge_range(self, lists, left, right):
        # 若只剩一個 linked list，直接回傳它
        if left == right:
            return lists[left]
        # 平分為兩半
        mid = (left + right) // 2
        # 遞迴合併左半
        l1 = self.merge_range(lists, left, mid)
        # 遞迴合併右半
        l2 = self.merge_range(lists, mid + 1, right)
        # 合併兩條已排序的 linked list
        return self.merge_two_lists(l1, l2)
    
    # 合併兩條已排序的 linked list
    def merge_two_lists(self, l1, l2):
        dummy = ListNode(0)
        current = dummy
        while l1 and l2:
            if l1.val < l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next
        # 將剩下的節點串接起來（一定只會剩l1或l2中的一個）
        current.next = l1 if l1 else l2
        return dummy.next  # 回傳真正的head節點
```
#### Accepted Pic
![螢幕擷取畫面 2025-04-07 231408](https://github.com/user-attachments/assets/6ab05071-bd07-4443-93cd-9cbe19657d4f)

