# week 4
### 50_Pow(x, n)
使用Divide and Conquer+Recursion
#### Code
```python
class Solution:
    def myPow(self, x: float, n: int) -> float:

        if n==0:
            return 1
        elif n==1:
            return x
        elif n<0:
            n = n(-1)
            return self.myPow(1/x, n)
        else:
            if n%2 == 0:
                half = self.myPow(x, n/2)
                return halfhalf
            else: 
                return x*self.myPow(x, n-1)
```
#### Accepted Pic
![螢幕擷取畫面 2025-04-07 215237](https://github.com/user-attachments/assets/c11bf88d-f518-42c7-93a0-ac3b62b7a65c)


