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
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        # 先用 node 指標檢查從 head 開始是否有足夠的 k 個節點
        node = head
        count = 0
        while node and count < k:
            node = node.next
            count += 1
        
        # 如果不足 k 個節點，就不進行反轉，直接回傳原始 head
        if count < k:
            return head

        # 開始反轉 k 個節點
        prev = None       # prev 用來記錄反轉後的前一個節點
        curr = head       # curr 是目前正在處理的節點
        for _ in range(k):
            nxt = curr.next   # 暫存下一個節點
            curr.next = prev  # 反轉指向
            prev = curr       # prev 往前移動
            curr = nxt        # curr 往前移動

        # head 是反轉後這一組的尾巴，要接上下一段反轉後的鏈表
        head.next = self.reverseKGroup(curr, k)

        # 返回這一組反轉後的頭節點（也就是 prev）
        return prev
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/fdd7a57b-7430-49b9-9235-23defb4c040e)




