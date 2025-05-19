# week 11
### 855_Exam Room
資料結構：heap(max-heap),dict<br>
演算法：Greedy
#### Code
```python
import heapq

class ExamRoom:

    def __init__(self, n: int):
        self.N = n
        self.heap = []
        self.avail_first = {} 
        self.avail_last = {}
        self._put_segment(0, self.N - 1)

    def _put_segment(self, first: int, last: int) -> None:
        if first == 0 or last == self.N - 1:
            priority = last - first
        else:
            priority = (last - first) // 2

        segment = [-priority, first, last, True]

        self.avail_first[first] = segment
        self.avail_last[last] = segment
        heapq.heappush(self.heap, segment)

    def seat(self) -> int:
        while True:
            _, first, last, is_valid = heapq.heappop(self.heap)
            if is_valid:
                del self.avail_first[first]
                del self.avail_last[last]
                break

        if first == 0:
            seat = 0
            if first != last:
                self._put_segment(first + 1, last)

        elif last == self.N - 1:
            seat = last
            if first != last:
                self._put_segment(first, last - 1)

        else:
            seat = first + (last - first) // 2
            if seat > first:
                self._put_segment(first, seat - 1)
            if seat < last:
                self._put_segment(seat + 1, last)

        return seat

    def leave(self, p: int) -> None:
        first = p
        last = p

        left = p - 1
        right = p + 1

        if left >= 0 and left in self.avail_last:
            segment_left = self.avail_last.pop(left)
            segment_left[3] = False

            first = segment_left[1]

        if right < self.N and right in self.avail_first:
            segment_right = self.avail_first.pop(right)
            segment_right[3] = False 

            last = segment_right[2]

        self._put_segment(first, last)

# Your ExamRoom object will be instantiated and called as such:
# obj = ExamRoom(n)
# param_1 = obj.seat()
# obj.leave(p)
 ```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/c8136b88-96c6-4583-9675-2b6a9d6dcee4)
