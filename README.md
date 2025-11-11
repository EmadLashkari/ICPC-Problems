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
squares = [x*x for x in nums if x % 2 == 1]  # [1, 