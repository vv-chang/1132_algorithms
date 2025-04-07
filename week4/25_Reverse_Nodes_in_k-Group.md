# week 4
### 25_Reverse_Nodes_in_k-Group
資料結構：Linked List<br>
演算法：Recursion + Pointer + Sliding Window
#### Code
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
        # head 為空 --> not head
        if not head or not head.next:
            return head
        else:
            first = head
            second = head.next
            # 要接到下一串的頭，而不是第二項的 next --> 這邊要用 Recursion
            first.next = self.swapPairs(second.next)
            second.next = first

            # 回傳換完之後的頭
            return second
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/fdd7a57b-7430-49b9-9235-23defb4c040e)




