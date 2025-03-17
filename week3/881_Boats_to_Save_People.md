# week 3
### 881_Boats_to_Save_People
#### Code
```python
class Solution:
    def numRescueBoats(self, people: List[int], limit: int) -> int:
        boat = 0
        light = 0
        heavy = len(people)-1
        
        people.sort()

        while light <= heavy:
            if (people[light] + people[heavy] <= limit):
                light += 1
            heavy -= 1
            boat += 1
            
        return boat
```
#### Accepted Pic
![w3_leetcode881](https://github.com/user-attachments/assets/063a619f-c783-46a3-a03c-6e0c708ec294)
