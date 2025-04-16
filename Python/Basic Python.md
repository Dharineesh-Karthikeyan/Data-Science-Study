# 🐍 Python for Data Science Interviews

This guide covers the **core Python topics** frequently asked in data science interviews. It focuses on practical Python features used in data analysis, machine learning, and data pipeline development.

---

Defintions: The term 'comprehension' computes a new list of the transformed elements, leaving the original list unchanged

## ✅ 1. Data Structures & Built-in Types

### 🔹 Lists, Tuples, Sets, Dictionaries
- **List**: Mutable, ordered collection
- **Tuple**: Immutable, ordered
- **Set**: Unordered, unique elements
- **Dict**: Key-value pairs

```python
my_list = [1, 2, 3]
my_tuple = (1, 2, 3)
my_set = {1, 2, 2, 3}       # {1, 2, 3}
my_dict = {'a': 1, 'b': 2}
```

### 🔹 Comprehensions
```python
squares = [x**2 for x in range(5)]
unique_lengths = {len(word) for word in ['a', 'bb', 'ccc']}
```

### 🔹 Special Collections
```python
from collections import Counter, defaultdict

c = Counter('banana')  # Count frequency
d = defaultdict(int)
d['missing'] += 1
```

---
<br></br>
## ✅ 2. Functions & Lambda Expressions

### 🔹 Defining and calling functions
```python
def add(a, b):
    return a + b
```

### 🔹 Lambda functions (anonymous)
```python
square = lambda x: x ** 2
```

### 🔹 *args and **kwargs
```python
def describe(*args, **kwargs):
    print(args)
    print(kwargs)

describe(1, 2, name="Alice", age=30)
```

---
<br></br>
## ✅ 3. List Comprehensions & Generators

### 🔹 List Comprehension
```python
[x**2 for x in range(5) if x % 2 == 0]
```

### 🔹 Generator Expression
```python
gen = (x**2 for x in range(5))  # memory-efficient
next(gen)
```

### 🔹 Yield
```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1
```

---
<br></br>
## ✅ 4. File I/O and Data Handling

### 🔹 Reading and writing files
```python
with open('file.txt', 'r') as f:
    content = f.read()

with open('output.txt', 'w') as f:
    f.write("Hello World")
```

### 🔹 Navigating files
```python
import os, glob

files = glob.glob('data/*.csv')
for f in files:
    print(os.path.basename(f))
```

---
<br></br>
## ✅ 5. Exception Handling

### 🔹 Try/Except Blocks
```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("Done")
```

### 🔹 Raising errors
```python
if age < 0:
    raise ValueError("Age must be positive")
```

---

## ✅ 6. Object-Oriented Programming (OOP)
<br></br>
### 🔹 Basic class
```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hello, {self.name}"
```

### 🔹 Inheritance
```python
class Student(Person):
    def __init__(self, name, major):
        super().__init__(name)
        self.major = major
```

---
<br></br>
## ✅ 7. Useful Built-in Functions

```python
list(zip([1, 2], ['a', 'b']))           # [(1, 'a'), (2, 'b')]
list(enumerate(['a', 'b']))             # [(0, 'a'), (1, 'b')]
any([True, False])                      # True
all([True, False])                      # False
sorted([3, 1, 2])                       # [1, 2, 3]
list(map(str, [1, 2]))                  # ['1', '2']
list(filter(lambda x: x > 2, [1, 2, 3]))# [3]
```

---

## ✅ 8. Regular Expressions
<br></br>
```python
import re

text = "Email: user@example.com"
match = re.search(r'\w+@\w+\.\w+', text)
email = match.group()  # 'user@example.com'

cleaned = re.sub(r'\d+', '', 'abc123')  # 'abc'
```

---
<br></br>
## ✅ 9. Datetime Handling

```python
from datetime import datetime, timedelta

now = datetime.now()
yesterday = now - timedelta(days=1)
formatted = now.strftime('%Y-%m-%d')
parsed = datetime.strptime('2023-01-01', '%Y-%m-%d')
```

### 🔹 With Pandas
```python
df['date'] = pd.to_datetime(df['date'])
df['dayofweek'] = df['date'].dt.day_name()
```

---
<br></br>
## ✅ 10. Working with APIs and JSON

### 🔹 Fetch and parse API response
```python
import requests

url = 'https://api.github.com/users/octocat'
res = requests.get(url)
data = res.json()
print(data['login'])  # 'octocat'
```

### 🔹 Navigating nested JSON
```python
for repo in data['repos']:
    print(repo['name'], repo['stargazers_count'])
```

---
<br></br>
## ✅ 11. Writing Clean & Modular Code

### 🔹 Use functions
```python
def clean_text(text):
    return text.lower().strip()
```

### 🔹 Use docstrings
```python
def calculate_mean(values):
    """Returns the mean of a list of numbers."""
    return sum(values) / len(values)
```

---
<br></br>
## ✅ 12. Testing & Debugging

### 🔹 Basic assertions
```python
assert 2 + 2 == 4
```

### 🔹 Try/Except with logging
```python
try:
    risky_operation()
except Exception as e:
    print(f"Error: {e}")
```

### 🔹 Debugging with `pdb`
```python
import pdb; pdb.set_trace()
```

---

