# ICPC Python Guide: Tips, Tricks, and Best Practices with Examples

A collection of best practices, tips, and tricks for solving competitive programming problems in Python, focusing on performance, handling large datasets, and efficient I/O.

## 1. Fast Input/Output Techniques

For problems with many I/O operations, standard methods can be too slow.

### Fast Input

Problem: Reading many integers efficiently.
Tip: Use sys.stdin.readline() to read lines faster than the standard input().

```python
import sys

# Assign the fast input method to a variable for convenience
input = lambda: sys.stdin.readline().rstrip()

# Example: Read a single integer
n = int(input())

# Example: Read multiple integers on a single line
a, b, c = map(int, input().split())

# Example: Read a list of integers
nums = list(map(int, input().split()))
```

Even faster for tokenized input:
```python
# Read entire stdin, split into tokens, and iterate
import sys
it = iter(sys.stdin.read().split())
II = lambda: int(next(it))
SS = lambda: next(it)

n = II()
arr = [II() for _ in range(n)]
```

### Fast Output

Problem: Printing 100,000 results.
Tip: Calling print() repeatedly is slow. Instead, collect outputs and print once.

```python
# FAST METHOD: Join a list of strings
output = []
for i in range(5):
    output.append(str(i))
print("\n".join(output))
```

Direct write (slightly faster):
```python
import sys
sys.stdout.write("\n".join(output))
sys.stdout.write("\n")
```

## 2. Data Type Conversion (Type Casting)

Input is almost always read as strings; convert to the correct type before calculations.

### String to Number (int, float)

Problem: Convert "123 45.6" to usable numbers.
Tip: Use int() and float(); use map() for lists.

```python
line = "123 45.6"
parts = line.split()

num_int = int(parts[0])      # 123
num_float = float(parts[1])  # 45.6

str_nums = ["10", "20", "30"]
int_nums = list(map(int, str_nums))  # [10, 20, 30]
```

### Number to String (str)

Problem: Build an output string from numbers for join().
Tip: Convert to str first.

```python
data = [100, 200, 300]
print(" ".join(map(str, data)))
```

### Character to ASCII and back (ord, chr)

Problem: Shift letters.
Tip: ord() for code point, chr() for character.

```python
char = 'a'
ascii_value = ord(char)        # 97
new_char = chr(ascii_value+3)  # 'd'
```

### Convert collections (list, tuple, set)

Problem: Deduplicate or use as dict key.
Tip: Use set() for uniqueness, tuple() for immutability.

```python
my_list = [1, 2, 2, 3, 1, 4]
unique = list(set(my_list))     # e.g., [1, 2, 3, 4]
key = tuple(my_list)            # usable as dict key
d = {key: "value"}
```

## 3. Choose the Right Data Structures

### set and dict (O(1) average operations)

Problem: Unique elements and frequency counting.
Tip: set for uniqueness; dict/Counter for counts.

```python
from collections import Counter
data = [1, 2, 2, 3, 4, 4, 4, 5]

unique_elements = set(data)
freq = Counter(data)  # Counter({4: 3, 2: 2, 1: 1, 3: 1, 5: 1})
```

### collections.deque (fast queue/stack)

Problem: Queue operations.
Tip: Use deque instead of list.pop(0).

```python
from collections import deque
q = deque([1, 2, 3])
q.append(4)
x = q.popleft()  # 1
```

### collections.defaultdict (clean aggregation)

Problem: Group items by key.
Tip: defaultdict avoids key checks.

```python
from collections import defaultdict
pairs = [('a', 1), ('b', 2), ('a', 3), ('b', 4)]
grouped = defaultdict(list)
for k, v in pairs:
    grouped[k].append(v)
# {'a': [1,3], 'b': [2,4]}
```

### heapq (priority queue)

Problem: k largest elements.
Tip: Use nlargest or maintain a size-k min-heap.

```python
import heapq
nums = [4, 1, 7, 3, 8, 5, 9, 2, 6]
k = 3
print(heapq.nlargest(k, nums))  # [9, 8, 7]
```

## 4. Code Optimization Strategies

### List/Dict/Set Comprehensions

Problem: Transform and filter quickly.
Tip: Comprehensions are concise and often faster.

```python
nums = [1, 2, 3, 4, 5]
squares = [x*x for x in nums if x % 2 == 1]  # [1, 9, 25]
```

### Local vs. Global Variables

Problem: Tight loops accessing globals.
Tip: Bind frequently used names to locals.

```python
def compute(arr):
    total = 0
    append = arr.append  # local binding example
    for i in range(5):
        append(i)
    return sum(arr)
```

### Minimize repeated function calls in loops

Problem: Calling len() repeatedly.
Tip: Hoist invariants.

```python
words = ["a"] * 1000000
n = len(words)
i = 0
while i < n:
    i += 1
```

## 5. Handling Large Datasets and Avoiding Timeouts

### Algorithm first

Problem: Two-sum existence.
Tip: Use a set in O(n) instead of O(n^2).

```python
def has_pair_sum(nums, k):
    seen = set()
    for x in nums:
        if k - x in seen:
            return True
        seen.add(x)
    return False
```

### Generators and streaming

Problem: Process a huge file without loading it all.
Tip: Iterate line-by-line.

```python
def process_large_file(path):
    with open(path, 'r') as f:
        for line in f:
            yield line.strip().upper()
```

### Memory-conscious patterns

Problem: Large accumulations.
Tip: Avoid building huge intermediate lists; prefer iterators.

```python
# Sum of integers from stdin tokens without constructing a list
import sys
tokens = sys.stdin.read().split()
total = 0
for t in tokens:
    total += int(t)
print(total)
```

## 6. Python-Specific Tips and Tricks

### Recursion depth

Problem: Deep DFS recursion.
Tip: Increase limit or convert to iterative.

```python
import sys
sys.setrecursionlimit(1_000_000)

# Iterative DFS template
def dfs(start, adj):
    stack, seen = [start], {start}
    while stack:
        u = stack.pop()
        for v in adj[u]:
            if v not in seen:
                seen.add(v)
                stack.append(v)
```

### Arbitrary-precision integers

Problem: Large powers/modular arithmetic.
Tip: Use pow with mod for speed.

```python
# Fast modular exponentiation
a, b, m = 2, 10**9, 1_000_000_007
print(pow(a, b, m))
```

### Efficient string building

Problem: Many concatenations.
Tip: Use join on list of chunks.

```python
chunks = []
for i in range(5):
    chunks.append(str(i))
s = "".join(chunks)
```

## 7. Contest-Useful Batteries Included

### Sorting tips

```python
# Sort by multiple keys: primary ascending, secondary descending
arr = [(3, 5), (3, 2), (1, 9)]
arr.sort(key=lambda x: (x[0], -x[1]))  # [(1,9), (3,5), (3,2)]
```

### Binary search (bisect)

```python
import bisect
a = [1, 3, 3, 5, 7]
i = bisect.bisect_left(a, 3)   # 1
j = bisect.bisect_right(a, 3)  # 3
count_of_3 = j - i             # 2
```

### Prefix sums and differences

```python
# Prefix sums
arr = [1, 2, 3, 4]
pref = [0]
for x in arr:
    pref.append(pref[-1] + x)
# sum of arr[l:r] = pref[r] - pref[l]

# Difference array for range increments
n = 5
diff = [0]*(n+1)
# add +v to [l, r)
l, r, v = 1, 4, 2
diff[l] += v
diff[r] -= v
# build back
a = [0]*n
cur = 0
for i in range(n):
    cur += diff[i]
    a[i] = cur
```

### Graph templates

```python
# Build adjacency list
n, m = 5, 4
edges = [(0,1), (1,2), (2,3), (3,4)]
adj = [[] for _ in range(n)]
for u, v in edges:
    adj[u].append(v)
    adj[v].append(u)  # remove if directed

# BFS
from collections import deque
def bfs(src):
    dist = [-1]*n
    dist[src] = 0
    dq = deque([src])
    while dq:
        u = dq.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                dq.append(v)
    return dist
```

### Dijkstra (with heapq)

```python
import heapq
def dijkstra(n, adj, src):
    INF = 10**18
    dist = [INF]*n
    dist[src] = 0
    pq = [(0, src)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist
```

### Disjoint Set Union (Union-Find)

```python
class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0]*n
    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x
    def union(self, a, b):
        a, b = self.find(a), self.find(b)
        if a == b: return False
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1
        return True
```

### Math helpers

```python
import math
# gcd/lcm
g = math.gcd(24, 36)                 # 12
l = 24 // g * 36                     # 72
# combinations modulo prime with pow
MOD = 1_000_000_007
def modinv(x): return pow(x, MOD-2, MOD)
```

### Coordinate compression

```python
a = [100, 500, 100, -10]
vals = sorted(set(a))          # [-10, 100, 500]
rank = {v:i for i, v in enumerate(vals)}
comp = [rank[x] for x in a]    # [1, 2, 1, 0]
```

## 8. Practical Checklist

- Read constraints; pick algorithms with correct complexity.
- Use fast I/O patterns for large inputs.
- Prefer set/dict/heapq/deque appropriately.
- Avoid deep recursion; prefer iterative.
- Use PyPy if allowed for speed.
- Hoist invariants; minimize per-iteration overhead.
- Batch outputs and string building.
- Test on provided samples; add edge cases.
