# week 7
### 815_Bus Routes
#### Code
```python
class Solution:
    def numBusesToDestination(self, routes: List[List[int]], source: int, target: int) -> int:
        if source == target:
            return 0
        
        # 1. 建立一個字典：station --> 可以搭到的公車
        station_to_buses = defaultdict(set)
        for bus_index, route in enumerate(routes):
            for station in route:
                station_to_buses[station].add(bus_index)
        
        # 2. 初始化，使用queue跟set
        queue = deque()
        visited_stations = set()  # 記錄已經到過的車站
        visited_buses = set()     # 記錄已經搭過的公車
        
        # 從起點開始
        queue.append((source, 0))  # (目前車站, 已搭公車數量)
        visited_stations.add(source)
        
        # 3. 開始BFS
        while queue:
            current_station, bus_count = queue.popleft()
            
            # 取得所有可以從這個車站搭乘的公車
            for bus in station_to_buses[current_station]:
                if bus in visited_buses:
                    continue  # 這輛公車已經搭過了，不要重複搭
                visited_buses.add(bus)
                
                # 看搭上這輛公車後，沿路可以到的所有車站
                for station in routes[bus]:
                    if station == target: # 搭這班就可以到達終點
                        return bus_count + 1 
                    if station not in visited_stations:
                        visited_stations.add(station)
                        queue.append((station, bus_count + 1))
        
        # 如果BFS結束還沒找到，代表無法抵達
        return -1
```
#### Accepted Pic
![image](https://github.com/user-attachments/assets/5012fb6d-a399-4cba-93eb-c7c267a06c7e)

