```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        
        candy = [1]*len(ratings)
        total = 0

        for i in range (0, len(ratings)-1, 1):
            if ratings[i] < ratings[i+1]:
                candy[i+1] = candy[i]+1
                
        for j in range (len(ratings)-1, 0 ,-1):
            if ratings[j] < ratings[j-1]:
                candy[j-1]= max(candy[j]+1, candy[j-1])
                

        candy = sum(candy)
        return(candy)
```
![w3_leetcode135](https://github.com/user-attachments/assets/4373da03-c92a-4f7c-b33d-31e19d8b3285)

