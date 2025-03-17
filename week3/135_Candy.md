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

![image](https://github.com/user-attachments/assets/01d88663-4af4-4089-96b0-7b28fce86ec6)
