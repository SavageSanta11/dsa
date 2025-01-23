### Kth Largest Element in a Stream
```python
```

#### Steps to solve the problem

#### Complexity

### Last Stone Weight
```python
import heapq
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        maxheap = [-x for x in stones]
        heapq.heapify(maxheap)

        while len(maxheap) >= 2:
            a = heapq.heappop(maxheap)
            b = heapq.heappop(maxheap)

            diff = (a - b)

            if diff != 0:
                heapq.heappush(maxheap, diff)

        if maxheap:
            return -maxheap[0]
        else:
            return 0

```

#### Steps to solve the problem
1. Maxheap to store stone weights
2. While heap length is >=2 , pop, check diff and push pack if needed
3. If there is still an element in that heap, return it else 0

#### Complexity
1. Time - O(n) to heapify. O(logn) for each pop. Approx n calls to push and pop so O(nlogn)
2. Space - O(n) for heap


### K closest points to origin
```python
import heapq
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        maxheap = [(-1*(x**2 + y ** 2), (x, y)) for (x, y) in points]
        heapq.heapify(maxheap)

        while len(maxheap) > k:
            heapq.heappop(maxheap)

        res = [item[1] for item in maxheap]
        return res


```

#### Steps to solve the problem
1. Use a maxheap to store (-dist, (x, y)) tuples. Keep pushing into heap and only pop while the heap size is >=k. Re
2. Remainining elements are the ones with lest distance

#### Complexity
1. Time - n calls to push n-k calls to pop. log(k) for each pop. Total is O( (2n-k)logk) = O(2nlogk - klogk) ~ O(nlogk)
2. Space - O(n)

#### Hint - for these problems with k closest or k largest, it typically means you just need to maintain a heap of size k. So don't be in a rush to heapify.

### K closest points to origin

#### Steps to solve the problem

#### Complexity


### Reorganize String
```python
class Solution:
    def reorganizeString(self, s: str) -> str:
        freq_map = {}
        for char in s:
            freq_map[char] = freq_map.get(char, 0) + 1
            
        max_heap = [(-freq, char) for char, freq in freq_map.items()]
        heapq.heapify(max_heap)
        
        res = []
        
        while len(max_heap) >= 2:
            freq1, char1 = heapq.heappop(max_heap)
            freq2, char2 = heapq.heappop(max_heap)
            
            res.extend([char1, char2])
            
            if freq1 + 1 < 0:
                heapq.heappush(max_heap, (freq1 + 1, char1))
            if freq2 + 1 < 0:
                heapq.heappush(max_heap, (freq2 + 1, char2))
                
        if max_heap:
            freq, char = heapq.heappop(max_heap)
            if -freq > 1:
                return ""
            res.append(char)
            
        return "".join(res)
```

#### Steps to solve the problem
1. Initialization:
    - Count the frequency of each character in the string.
    - Populate the max heap with these frequencies.
2. Processing Each Character:
    - Pop the top two characters from the max heap (i.e., the ones with the highest frequency).
    - Append these two characters to the result string.
    - Decrement their frequencies and re-insert them back into the max heap.
    - If only one character remains in the heap, make sure it doesn't exceed half of the string length, otherwise, return an empty string.

3. Wrap-up:
    - If there's a single remaining character with a frequency of 1, append it to the result.
    - Join all the characters to return the final reorganized string
  
#### Complexity
Approach: O(nlogk)

- O(n) for counting the frequency of each character in the string. Here, n is the length of the string.
- O(klogk) for building the max heap, where k is the number of unique characters in the string.
- The heap operations (insertion and deletion) would require logk time each. In the worst-case scenario, you would be doing these operations n times (once for each character in the string).

- Space is O(k)
### Task Scheduler
```python
from collections import Counter
import heapq

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:

        count = Counter(tasks)
        maxHeap = [-cnt for cnt in count.values()]
        heapq.heapify(maxHeap)

        time = 0
        q = deque() #[-cnt, when it can be added next]

        while maxHeap or q:
            time+= 1

            if maxHeap:
                cnt = 1 + heapq.heappop(maxHeap)
                if cnt != 0:
                    q.append([cnt, time + n])

            if q and q[0][1] == time: ## if some task in the queue can now be completed because its idle time is over
                heapq.heappush(maxHeap, q.popleft()[0]) 
        return time
        
```

#### Steps to Solve the Problem

1. **Count Frequencies**:  
   - Use `Counter` to count the occurrences of each task.

2. **Max Heap**:  
   - Store the negative counts of tasks in a max heap (`maxHeap`) so that the most frequent task is processed first.

3. **Queue for Cooldown**:  
   - Use a queue (`q`) to track tasks that are cooling down.  
   - Each entry in `q` stores:
     - The remaining count of the task.  
     - The time when it can be re-added to the heap.

4. **Simulate Time**:  
   - Increment `time` for every CPU cycle.  
   - If `maxHeap` is not empty:
     - Pop a task, decrement its count, and if the count is still non-zero, add it to the `q` with its cooldown time.  
   - If the front of `q` has completed its cooldown (`q[0][1] == time`), re-add it to `maxHeap`.

5. **Stop Condition**:  
   - Continue the simulation until both `maxHeap` and `q` are empty.

6. **Return Time**:  
   - The `time` variable reflects the total CPU cycles required.

---

#### Key Components of the Code

- **`maxHeap`**: Prioritizes tasks with the highest frequency for execution.  
- **`q` (Queue)**: Ensures tasks respect the cooldown constraint.  
- **Simulation of Time**: Tracks each CPU cycle.

---

#### Complexity

- **Time Complexity**:  O(nlogk)
  where \(n\) is the number of tasks and \(k\) is the number of unique tasks.

- **Space Complexity**:  O(k) for storing tasks in the heap and queue.
