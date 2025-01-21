
### Task Scheduler
```python
from collections import Counter
import heapq

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        freq = Counter(tasks)
        counts = [-v for k, v in freq.items()]
        heapq.heapify(counts)

        time = 0

        cooldown_queue = deque()

        while cooldown_queue or counts:
            time += 1

            if counts:
                task = heapq.heappop(counts)
                curr_count = task + 1 # + 1 because the counts are negative
                if curr_count != 0:
                    cooldown_queue.append((curr_count, time + n))

            if cooldown_queue and cooldown_queue[0][1] == time: # is some tasks cooldown is over
                counts.append(cooldown_queue.popleft()[0])

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
