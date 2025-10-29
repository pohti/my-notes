# Python

## Basic Level Questions

### 1. What are the key features of Python?

**Answer:** Python is interpreted, dynamically typed, object-oriented, has simple syntax, extensive standard library, cross-platform compatible, supports multiple programming paradigms, and has automatic memory management.

### 2. What is the difference between a list and a tuple?

**Answer:**

- **List:** Mutable (can be changed), uses square brackets `[]`, slower, more memory
- **Tuple:** Immutable (cannot be changed), uses parentheses `()`, faster, less memory

```python
my_list = [1, 2, 3]
my_list[0] = 10  # Works

my_tuple = (1, 2, 3)
my_tuple[0] = 10  # Error!

```

### 3. What is the difference between `==` and `is`?

**Answer:**

- `==` checks if values are equal
- `is` checks if they are the same object in memory

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)  # True (same values)
print(a is b)  # False (different objects)
print(a is c)  # True (same object)

```

### 4. Explain mutable vs immutable data types

**Answer:**

- **Mutable:** Can be changed after creation (list, dict, set)
- **Immutable:** Cannot be changed after creation (int, float, str, tuple, bool)

```python
# Mutable
my_list = [1, 2, 3]
my_list.append(4)  # Works

# Immutable
my_string = "hello"
my_string[0] = "H"  # Error!

```

### 5. What are Python's built-in data types?

**Answer:**

- **Numeric:** int, float, complex
- **Sequence:** list, tuple, range, str
- **Mapping:** dict
- **Set:** set, frozenset
- **Boolean:** bool
- **None:** NoneType

## Intermediate Level Questions

### 6. Explain list comprehension with an example

**Answer:** List comprehension provides a concise way to create lists.

```python
# Traditional way
squares = []
for i in range(10):
    squares.append(i ** 2)

# List comprehension
squares = [i ** 2 for i in range(10)]

# With condition
even_squares = [i ** 2 for i in range(10) if i % 2 == 0]

```

### 7. What is the difference between `append()` and `extend()`?

**Answer:**

- `append()` adds a single element
- `extend()` adds multiple elements

```python
list1 = [1, 2, 3]
list1.append([4, 5])
print(list1)  # [1, 2, 3, [4, 5]]

list2 = [1, 2, 3]
list2.extend([4, 5])
print(list2)  # [1, 2, 3, 4, 5]

```

### 8. What are *args and **kwargs?

**Answer:**

- `args` allows passing variable number of positional arguments
- `*kwargs` allows passing variable number of keyword arguments

```python
def example(*args, **kwargs):
    print("Args:", args)
    print("Kwargs:", kwargs)

example(1, 2, 3, name="Alice", age=25)
# Args: (1, 2, 3)
# Kwargs: {'name': 'Alice', 'age': 25}

```

### 9. Explain lambda functions

**Answer:** Lambda functions are anonymous, single-expression functions.

```python
# Regular function
def add(x, y):
    return x + y

# Lambda function
add = lambda x, y: x + y

# Common use with map, filter
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
even = list(filter(lambda x: x % 2 == 0, numbers))

```

### 10. What is the difference between `remove()`, `pop()`, and `del`?

**Answer:**

```python
my_list = [1, 2, 3, 4, 5]

# remove() - removes first occurrence of value
my_list.remove(3)  # [1, 2, 4, 5]

# pop() - removes and returns item at index (default: last)
item = my_list.pop(1)  # Returns 2, list: [1, 4, 5]

# del - removes item at index or entire variable
del my_list[0]  # [4, 5]

```

### 11. Explain dictionary methods: `get()`, `setdefault()`, and `update()`

**Answer:**

```python
d = {"a": 1, "b": 2}

# get() - safe access with default
print(d.get("c", 0))  # 0 (doesn't error)

# setdefault() - set only if key doesn't exist
d.setdefault("c", 3)  # d: {"a": 1, "b": 2, "c": 3}
d.setdefault("a", 10)  # d unchanged (key exists)

# update() - merge dictionaries
d.update({"d": 4, "e": 5})

```

### 12. What is the Global Interpreter Lock (GIL)?

**Answer:** The GIL is a mutex that protects access to Python objects, preventing multiple threads from executing Python bytecode simultaneously. This means true parallelism isn't possible with threads, but multiprocessing can be used instead.

## Advanced Level Questions

### 13. Explain decorators with an example

**Answer:** Decorators modify or enhance functions without changing their code.

```python
def uppercase_decorator(func):
    def wrapper():
        result = func()
        return result.upper()
    return wrapper

@uppercase_decorator
def greet():
    return "hello world"

print(greet())  # "HELLO WORLD"

```

### 14. What are generators and why use them?

**Answer:** Generators are functions that yield values one at a time, saving memory.

```python
# Regular function - stores all in memory
def get_numbers(n):
    return [i for i in range(n)]

# Generator - yields one at a time
def get_numbers_gen(n):
    for i in range(n):
        yield i

# Usage
gen = get_numbers_gen(1000000)  # Doesn't create list
for num in gen:
    print(num)  # Process one at a time

```

### 15. Explain the difference between deep copy and shallow copy

**Answer:**

```python
import copy

original = [[1, 2, 3], [4, 5, 6]]

# Shallow copy - copies references
shallow = copy.copy(original)
shallow[0][0] = 99
print(original)  # [[99, 2, 3], [4, 5, 6]] - changed!

original = [[1, 2, 3], [4, 5, 6]]

# Deep copy - copies everything
deep = copy.deepcopy(original)
deep[0][0] = 99
print(original)  # [[1, 2, 3], [4, 5, 6]] - unchanged

```

### 16. What is the difference between `@staticmethod` and `@classmethod`?

**Answer:**

```python
class MyClass:
    class_var = "I'm a class variable"

    def instance_method(self):
        return "Instance method", self

    @classmethod
    def class_method(cls):
        return "Class method", cls.class_var

    @staticmethod
    def static_method():
        return "Static method - no access to instance or class"

```

### 17. Explain context managers and the `with` statement

**Answer:** Context managers handle resource setup and cleanup automatically.

```python
# Without context manager
file = open("data.txt", "r")
data = file.read()
file.close()  # Must remember to close

# With context manager
with open("data.txt", "r") as file:
    data = file.read()
# File automatically closed

# Custom context manager
class MyContext:
    def __enter__(self):
        print("Entering context")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Exiting context")

with MyContext() as ctx:
    print("Inside context")

```

## Coding Challenges

### 18. Reverse a string

```python
def reverse_string(s):
    return s[::-1]

# Alternative
def reverse_string(s):
    return ''.join(reversed(s))

```

### 19. Check if a string is a palindrome

```python
def is_palindrome(s):
    s = s.lower().replace(" ", "")
    return s == s[::-1]

print(is_palindrome("A man a plan a canal Panama"))  # True

```

### 20. Find duplicate elements in a list

```python
def find_duplicates(lst):
    seen = set()
    duplicates = set()
    for item in lst:
        if item in seen:
            duplicates.add(item)
        seen.add(item)
    return list(duplicates)

# Alternative using Counter
from collections import Counter
def find_duplicates(lst):
    counts = Counter(lst)
    return [item for item, count in counts.items() if count > 1]

```

### 21. FizzBuzz

```python
def fizzbuzz(n):
    for i in range(1, n + 1):
        if i % 15 == 0:
            print("FizzBuzz")
        elif i % 3 == 0:
            print("Fizz")
        elif i % 5 == 0:
            print("Buzz")
        else:
            print(i)
```

### 22. Fibonacci sequence

```python
# Recursive
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Iterative (more efficient)
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a

# Generator
def fibonacci_gen():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b
```

### 23. Merge two sorted lists

```python
def merge_sorted_lists(list1, list2):
    result = []
    i = j = 0

    while i < len(list1) and j < len(list2):
        if list1[i] < list2[j]:
            result.append(list1[i])
            i += 1
        else:
            result.append(list2[j])
            j += 1

    result.extend(list1[i:])
    result.extend(list2[j:])
    return result
```

### 24. Remove duplicates from a list while preserving order

```python
def remove_duplicates(lst):
    seen = set()
    result = []
    for item in lst:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result

# Using dict (Python 3.7+, preserves order)
def remove_duplicates(lst):
    return list(dict.fromkeys(lst))
```

## Tips for Python Interviews

1. **Understand time/space complexity** - Know Big O notation
2. **Practice coding without an IDE** - Use pen and paper
3. **Explain your thinking** - Talk through your approach
4. **Test your code** - Walk through examples
5. **Know standard library** - collections, itertools, functools
6. **Ask clarifying questions** - Understand requirements first
7. **Optimize after it works** - Working solution first, then improve
8. **Know Pythonic idioms** - List comprehensions, context managers, etc.