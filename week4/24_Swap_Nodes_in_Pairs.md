# week 4
### 24_Swap_Nodes_in_Pairs
資料結構：Linked List<br>
演算法：Recursion + Pointer
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
![image](https://github.com/user-attachments/assets/7eba6b0d-1e38-4e04-b822-9db248d1bf09)

