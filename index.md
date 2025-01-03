---
layout: home
title: Notes
---

## Control Flow, Variables

### Loops

#### For Loops

For loops loop over any iterator:

```python
arr = [1, 2, 3]
for item in arr:
  print(arr)
```

##### Range Function
We can use range to produce an iterator over range of numbers:

```python
# start inclusive, end exclusive, last argument step size
range(3) # iterates from 0...2
range(1, 3) # iterates from 1...2
range(2, 0, -1) # iterates from 2...1
```

##### Enumerate Function
Turns an iterator into another iterator of tuples of (index, value of original iterator):

```python
arr = ["bill", "jeff"]

enumerate(arr) # (0, bill), (1, jeff)
enumerate(arr, start=1) # (1, bill), (2, jeff)
```

#### While Loops
While loops work like any other language

```python
while cond():
  print("loop")
```

#### Loop Control
- `break`: leave loop early
- `continue`: go to start of next loop iteration
- Loop `else` clause: executed if loop runs to completion (no `break` statement)

```python
for i in range(5):
  print(i)
else:
  print("Ran to completion") # will print

for i in range(5):
  break
else:
  print("Ran to completion") # won't print
```

### Assignment
#### Walrus Operator
`:=` allows us to assign a value to a variable as part of an expression:

```python
with open("file.txt") as f:
  while (line := f.readline()):
    print(line.strip())
```

#### Multiple Variable Assignment
Can assign variables like this:

```python
a, b = 1, 3
a, b = b, a # swap values
```

#### Numerical Variable Assignment
Can use an underscore as a separator in large numbers: `population = 1_000_000`

### Ternary Operators
In python, they work like this:

```python
n = 5
print("Even" if n % 2 == 0 else "Odd")
```

### Expressions
- Math operators: all normal, exponentiation: `a ** b` (equals a^b)
- Logical: `and`, `or`, `not`
- Equality: `==`, `!=`
- Comparison: `>=`, `<=`, `>`, `<`, can be combined: `1 < x < 5`

### Structural Pattern Matching

Their most basic form work like switch statements:

```python
match status:
  case 400:
    return "Bad request"
  case 404:
    return "Not found"
  case _:
    return "Something's wrong with the internet"
```

Can combine several options into one 'pattern' with `|`:

```python
case 401 | 403 | 404:
  return "Not allowed"
```

Can unpack/assign to variables in a pattern:

```python
match point:
  case (0, 0):
    print("Origin")
  case (0, y):
    print(f"Y={y}")
  case (x, 0):
    print(f"X={x}")
  case (x, y):
    print(f"X={x}, Y={y}")
# we can now see that the 'default' case is just unpacking whatever value we have
#   into a variable, which would always match
  case _: 
    raise ValueError("Not a point")
```

We can also add an if statement to a pattern (a *guard*), which only lets the pattern match if the statement is true:

```python
match point:
    case Point(x, y) if x == y:
        print(f"Y=X at {x}")
    case Point(x, y):
        print(f"Not on the diagonal")
```

To do unpacking with a custom class like `Point` above, we need to tell Python what attributes to unpack into the variables:

```python 
class Point:
  __match_args__ = ("x", "y")
  def __init__(self, x, y):
    self.x = x
    self.y = y
```

## Functions
- Functions can be declared within each other
- By convention functions are declared before they are used

### Function Arguments

```python
def func(a, b=10, *args, c, d=5, **kwargs)
```

- There are 6 main types of arguments, and they need to be defined the below order:
  - Positional arguments (`a`): `func(12, ...`
    - Can also be specified by name, but must still be in order: `func(a=12, ...`
  - Positional arguments with default values (`b`): can be omitted in call
  - Variable-length positional arguments (`*args`): captures additional positional args in call
  - Keyword arguments (`c`): `func(12, c=7, ...`
    - Must be specified by name
    - Can be passed out of order: `func(12, d=5, c=7. ...`
    - To force callers to pass as keywords, must be defined after `*args`, or just `*` (if we're not going to do anything with the captured positional args)
  - Keyword arguments with default values (`d`): can be omitted in call
  - Variable-length keyword arguments (`*kwargs`): captures additional keyword args in call
- Additional type: positional only arguments
  - To prevent positional arguments from being specified by name, they must be defined before a `/`

Example: `a`, `b` are positional arguments that cannot be specified by name, `c` can

```python
def func(a, b, /, c)
```

### Lambda Functions
Lambda functions are small anonymous functions (limited to one expression):

```python
# Example uses of a lambda
do_something(lambda a, b: a+b)
pairs.sort(key=lambda pair: pair[1])
```

## Useful Builtin Functions
- `ord(x)`: gets unicode character code for x
- `quotient, remainder = divmod(a, b)`: does a / b, returning quotient and remainder
- `sorted(x)`: sorts an iterator
- `all(x)`, `any(x)`: returns true if all elements in iterator True, or any are (respectively)
- `filter(func, x)`: constructs an iterator from elements of iterable `x` for which `func` returned true
  - Ex: `filter(lambda x: x % 2 == 0, arr)`
- `map(func, x)`: constructs an iterator by applying `func` to each element of the iterable `x`
  - Ex: `map(lambda x: x ** 2, arr)`
- `len(x)`: returns length of iterator
- `sum(x)`: sums items in iterator
- `min(x)`, `max(x)`: returns min/max value in iterator
- `zip(*iterators, strict=False)`: returns an iterator of tuples, where the i-th tuple contains the i-th element from each of the argument iterables
  - Will stop after shortest of iterators exhausted, if `strict=True` will instead raise `ValueError`

## Data Structures
### String
Python strings are immutable sequences of characters

```python
s = "Hello, World! Hello."

'Hello' in s # True if substring in string
s.count("Hello") # Count occurrences of substring

s.find("Hello") # Returns starting index of first occurrence of substring, -1 if not found
s.rfind("Hello") # Returns starting index of last occurrence

s.startswith("Hello") # True if string starts with substring (also: endswith)
s.removeprefix("Hello") # Remove that substring (also: removesuffix)

", ".join([1, 2, 3]) # Join iterable using string

s.replace("Hello", "Bye") # Replace a sustring with another
s.split(",") # Split the string by the substring
s.splitlines() # Split the string by line breaks

s.strip() # Remove leading & trailing whitespace (also: lstrip, rstrip)
s.upper() # Make string uppercase (also: lower)
```

#### Regex
The `re` module provides Regex support.

- **Modifying the string - common**
  - `re.split(r"pattern", string)`: return `string` split by matches of `pattern`
  - `re.sub(r"pattern", repl, string)`: return `string` with all matches of `pattern` replaced with `repl`
- Finding a single match
  - `re.search(r"pattern", string)`: return first `Match` of `pattern` anywhere in `string`, or None
  - `re.match(r"pattern", string)`: return `Match` of `pattern` beginning at start of `string`, or None
  - `re.fullmatch(r"pattern", string)`: return `Match` if whole `string` matches `pattern`, or None
    - Essentially: `re.fullmatch(r"pattern")` == `re.search(r"^pattern$")`
- Using a `Match`
  - `match.group()`: Returns entire matched string (contents of first capturing group)
  - `match.groups()`: Return contents of all capturing groups in match
- Finding all matches
  - `re.findall(r"pattern", string)`: return all matches as a list of strings or tuples
    - If capturing groups in pattern will be tuple of their contents, else a string of the whole match

##### Common Regex
- `r"[^a-zA-Z]+"`: Match non-letters (good for splitting by word)
- `r"\d+"`: Match integer

### List
Python lists are mutable, ordered collections of items, backed by a resizable array

```python
my_list = [1, 2, 3]

my_list.append(4)
my_list.extend([5, 6]) # append 5, 6
my_list.insert(0, 7) # insert 7 at index 0 (tc: o(n))

my_list.remove(4) # remove first item equal to 4
my_list.pop() # remove from end
my_list.pop(0) # remove from index 0
del my_list[1] # remove (and you don't care what the value is)

my_list.clear()
my_list.index(3) # get first index of item equal to 3
my_list.count(3) # count items equal to 3
my_list.reverse() # reverse list

3 in my_list # inefficient, o(n) check
```

#### List Comprehension

List comprehensions are made of 2 or 3 components:

```python
fizz_buzz_squares = [
    num ** 2                             # Data transformation (what actually goes in list)
    for num in range(10)                 # Data source (any iterable)
    if (num % 3 == 0) or (num % 5 == 0)  # Data filter (optional)
]
```

Often written on one line, like:

```python
element_names = [ el["name"] for el in elements ]
```

Can do more complicated comprehensions, although this gets confusing:

```python
flattened = [exc for sub in exceptions for exc in sub] 
```

#### List Slicing, Indexing
- Can index from the start or back (zero-indexed):

```python
arr = [1, 2, 3]
print(arr[0]) # 1
print(arr[-1]) # 3
```

- Can 'slice' a portion of an array
  - First number: start index (inclusive)
  - Second number: end index (exclusive)
  - Third number: step size 

```python
arr = [1, 2, 3, 4, 5]
print(arr[0:2]) # [1, 2]
print(arr[0:4:2]) # [1, 3]
```

- To do indexing on custom classes, need to implement several dundermethods

### Tuple
Python tuples are immutable, ordered collections of items, backed by a fixed-size array.

```python
my_tuple = (1, 2, 3)
another_tuple = (1, ) # if tuple one item, needs trailing comma

# Tuples support all methods from lists that don't involve mutation 
```

Because tuples are immutable they can be serialized which makes them useful for keying `dict`s, etc.

#### Getting the 'inverse' of an iterable of tuples
- We can get inverse of a iterable of tuples like: `new_list = zip(*my_list)`
  - Turns `my_list = [(1, "a"), (2, "b")]` into `new_list = [(1, 2), ("a", "b")]`
- Explanation:

```python
# say we have data like this
data = [(1, "a"), (2, "b"), (3, "c")]
# zip(*data) turns into:
ndata = zip((1, "a"), (2, "b"), (3, "c"))
# zip produces tuples with an item from each input tuple in each one
ndata = [(1, 2, 3), ("a", "b", "c")]
```

#### Named Tuple
`namedtuple()` creates tuple objects with named fields so fields can be accessed by index or name

```python
Point = namedtuple("Point", ["x", "y"])

# Can assign values using positional or keyword args
p = Point(11, y=22)

# Equivalent
print(p[0] + p[1])
print(p.x + p.y)
```

#### Tuple Comparisons
- Tuples are compared by comparing their contents left to right, stopping once unequal elements found
- This makes them great for using with sorts and heapqs

### Set
Python sets are mutable, unordered collections of unique items, backed by a hash table.

```python
my_set = {1, 2, 3}
empty_set = set() # empty_set = {} would make a dict

my_set.add(4)
my_set.update([5, 6]) # add 5, 6

my_set.remove(4) # remove item equal to 4, KeyError if not found
my_set.discard(7) # remove item equal to 4, does nothing not found
my_set.pop() # remove a random item
my_set.clear()

3 in my_set # efficient, o(1) check

a = {1, 2, 3}
b = {3, 4, 5}

a.union(b) # return combined items 
a | b # union shorthand
a.intersection(b) # return common items
a & b # intersection shorthand
a.difference(b)  # return items in `a` not in `b`
a - b # difference shorthand
a.symmetric_difference(b)  # return items in `a` or `b` but not both
a ^ b # symmetric difference shorthand

a.issubset(b) # True if all items in a also in b
a.issuperset(b) # True if all items in b also in a
a.isdisjoint(b) # True if a and b share no items
```

#### Aside: how does python generate object hashes?

- Hash tables need object hashes ([learn why](https://thepythoncorner.com/posts/2020-08-21-hash-tables-understanding-dictionaries/))
- Python uses the `hash(obj)` function to generate the hash of `obj`
- For custom objects, you may need to implement `__hash__`
  - By default, python will use a hash function based on the object's ID
  - If `__eq__` is specified, you need to implement `__hash__` otherwise a `TypeError` is raised
    - A hash function should produce the same output if two objects are equal, so since we overrode the default `__eq__`, we can't use the default `__hash__` anymore
- You should only be able to hash immutable objects, otherwise the invariant that the same object produces the same hash would be broken

### Dict
Python dictionaries are mutable, collections of key-value pairs, backed by a hash table.

```python
my_dict = {"a": 1, "b": 2, "c": 3}

my_dict["a"]  # get value for key 'a'
my_dict["a"] = 10  # update value for key 'a'

my_dict.pop("b")  # remove key 'b' and return its value
del my_dict["c"]  # remove key 'c'
my_dict.clear()

my_dict.keys() # get all keys
my_dict.values() # get all values
my_dict.items() # get all key-value pairs

"a" in my_dict  # efficient, o(1) check
my_dict.get('e', 0) # get value for key 'e', return 0 if not found

my_dict |= {"e": 5} # merge another dict
my_dict.update({"e": 2}) # overwrite keys from another dict
my_dict["e"] # = 2
```

#### Dict Comprehension

Like with lists, dict comprehensions are made of 2 or 3 components:

```python
parts = ["CPU", "GPU", "Motherboard"]
stocks = [15, 8, 12]

inventory = [
    part: qty                            # Data transformation (what actually goes in dict)
    for part, qty in zip(parts, stocks)  # Data source (any iterable)
    if qty > 0                           # Data filter (optional)
]
```

#### OrderedDict
- Dictionaries are ordered by default: first key inserted is first in `.keys()` ordering
- OrderedDict provides methods to reorder keys in a dict

```python
from collections import OrderedDict

or_dict = OrderedDict({"a": 1, "b": 2, "c": 3})

# pops (key, value) pair corresponding to last key inserted
or_dict.popitem(last=True) 

# moves key 'a' to end of dictionary (would be returned first from above)
or_dict.move_to_end("a", last=True) 
```

#### defaultdict
- When a key is accessed that doesn't exist, a defaultdict provides a default value by calling a default factory
- The default factory is often a class like list, int, or set, which creates a new instance when called

```python
from collections import defaultdict

my_dict = defaultdict(list)

my_dict["abc"] # = []
```

#### Counter
- A counter counts hashable objects and can be interacted with just like a dict
  - Its constructor either takes an iterable (ex: list of items to count) or a mapping where the keys are the items and the values are the counts

```python
from collections import Counter

# Equivalent
c = Counter([1, 1, 2, 3, 4])
c = Counter({1: 2, 2: 1, 3: 1, 4: 1})

c.update([2, 2, 3]) # add items to count (can iterable or mapping, like constructor)
c.subtract(other_count) # subtract other_count

c.most_common(2) # return 2 highest counts as (item, count) pairs
c.total() # return total count as an integer
```

### deque (pronounced deck)
Deques are mutable, ordered collections of items, backed by a doubly-linked list for O(1) insertion/removal from either sides

```python
from collections import deque

my_deque = deque([1, 2, 3])

# Insert
my_deque.append(4)
my_deque.appendleft(0)
# also: extend(), extendleft()

# Remove
my_deque.pop()
my_deque.popleft()

# Rotate: move 2 steps right (insert 2 elements from right, insert on left)
my_deque.rotate(2)

# Rotate: move 2 steps left (insert 2 elements from left, insert on right)
my_deque.rotate(-2)

# Deques support all methods from Python lists, but methods related to random access or insertion/removal from the middle of the deque are O(n)
```

- A `maxlen` can be specified in the constructor: `deque([1, 2, 3], maxlen=3)`
- When this length is reached and an element is inserted, an element is removed from the opposite end

### heapq
A heapq is a implementation of a min-heap backed by a Python list.

```python
my_heap = []

# below: O(logn) operations
heapq.heappush(my_heap, 2) # add 2 to heap
heapq.heappop(heap) # return smallest value in heap

# Turn a list into a heap in linear time
new_heap = [3, 1, 2]
heapq.heapify(new_heap)

# Return the largest/smallest 2 elements from an unsorted list
unsorted = [3, 1, 2]
heapq.nlargest(2, unsorted)
heapq.nsmallest(2, unsorted, key=lambda x: x**2)
```

#### Changing heap ordering
- A heapq will always use `a < b` to produce a min-ordering: what if we need a different ordering?
- **Transform data before/after:** if we want a heap with the largest number on top, we can just multiply numbers by -1 before we insert & after we remove
- **Insert as tuples:** due to how comparison of tuples work, we can use our desired ordering as the first component, then the number of insertions so far (to preserve stability), then our actual item
  - This looks like this: `(ordering, insert_count, item)`
  - We just look at the third component when popping from our heap
  - We need `insert_count`, or if `ordering` was the same for two things in our heap, it would compare the `item`s to break the tie; in a stable sort it should break ties with insertion order
- **Override `__lt__(other)`**: we can write a `__lt__` which returns True if we want this object to come before `other`

### Decimal
- Decimal is a high-precision implementation of floating-point arithmetic that uses base-10
- For comparison, Python's binary floating point `float` would produce: `1.1 + 2.2 = 3.3000000000000003`
- To create a `Decimal` instance, pass a number or string to its constructor
  - Decimal is immutable

```python
Decimal(10)
Decimal("3.14")
```

- Decimal supports most arithmetic operators supported by `int`, `float`, etc.

#### Arithmetic Context
Contexts are environments for arithmetic operations which govern precision, rules for rounding, etc.

Example: set context for current thread

```python
from decimal import Decimal, getcontext

getcontext().prec = 5

result = Decimal("1.123456") + Decimal("2.654321")
print(result) # = 3.7778
```

Example: set context for a block of code using a context manager

```python
from decimal import Decimal, localcontext

num1 = Decimal("1.123456")
num2 = Decimal("2.654321")

with localcontext() as ctx:
    ctx.prec = 3
    print(num1 + num2) # = 3.78 
```

### Fraction
- Fraction enables precise rational number arithmetic
  - Rational number: any number which can be represented as p/q where p & q are integers
- Supports most arithmetic operators

```python
from fractions import Fraction

Fraction(16, -10) # 16/-10
Fraction(123) # 123/1
Fraction("3/7") # 3/7
```

### Time
The `Time` module works in raw, very precise times measured as time since UNIX epoch.

```python
time.time() # Current time in seconds
time.time_ns() # Nanoseconds

time.sleep(s) # Sleep thread for s seconds
```

### DateTime
- All DateTime classes other than delta can be naive or aware: aware objects pay attention to timezone and are more complicated to use, so use naive unless you need to stuff with multiple timezones
  - Below examples are for naive
- Date represents a calendar date (year, month, day):

```python
from datetime import date

d = date(2024, 12, 27) # Create a date
print(date.today()) # Get current date
```

- Time represents a time (hour/minute/second):

```python
from datetime import time

t = time(14, 30, 45) # Create a time
print(t.hour, t.minute) # Access attributes
```

- Datetime combines the two:

```python
from datetime import datetime

dt = datetime(2024, 12, 27, 14, 30, 45) # Create a datetime
dt2 = datetime.fromtimestamp(111100) # Create a datetime from UNIX time

print(datetime.now()) # Get current date and time
```

- Timedelta represents a duration/difference between dates/times:

```python
from datetime import timedelta

future = dt + timedelta(days=5) # Add 5 days

td = timedelta(days=1, minutes=1)
print(td.total_seconds()) # Get duration in seconds
```

- We can easily parse and format dates, times, and datetimes:

```python
dt.strftime("%Y-%m-%d %H:%M:%S") # Format to string
new_dt = datetime.strptime("2024-12-27", "%Y-%m-%d") # Parse from string

dt.toisoformat() # Format to ISO format
dt.fromisoformat("2024-12-27") # Parse from ANY ISO format
```

- Classes from the DateTime module also support timezones with ZoneInfo, making them *aware*:

```python
from datetime import datetime
from zoneinfo import ZoneInfo

dt = datetime.now(tz=ZoneInfo("UTC")) # Current UTC time
```

## Classes
### Class Basics

```python
class MyClass:
  """A simple example class""" # docstring, accessible with MyClass.__doc__

  i = 123 # class attribute (shared between instances)

  def __init__(self, foo): # constructor
    self.data = [foo] # Instance attribute

  def f(self): # method
    return self.data
```

```python
x = MyClass(2)
print(x.f())
```

### Calling Methods
- Assume `x = SomeClass()` , if you do `x.foo()`, the instance object will be passed as the the first argument to the function
  - Conversely, if you did `SomeClass.foo()`, no instance is passed
  - So `SomeClass.foo(x)` is the same as `x.foo()`
- So, methods are not `static` or `instance` like in other languages, nothing stops a user from calling them in one context or another (see method decorators for exceptions)
  - Additionally, naming the first attribute `self` is nothing but convention

### Dunder methods
- Allow instances of class to interact with built-in Python operators and methods
- Common dunder methods
  - `__init__` - called to initialize a instance of a class
  - `__repr__` - unambiguous string representation of instance
  - `__str__` - 'pretty' string representation of instance
  - `__eq__(other)` - True if this instance is equal to `other`, called when `==` used
  - `__lt__(other)` - True if this instance is less than `other`, called when `<=` used
  - `__getitem__(key)`, `__setitem__(key, val)`, `__delitem__(key)` - called when `obj[key]` used
  - `__contains__(item)` - True if item 'in' instance, called when `item in instance` used 
  - `__enter__`, `__exit__` - Called when enter/exit context manager, called when `with obj` used 

### Method Decorators
- We can use the `@property` decorator to make getters and setters that appear like normal attributes of a class:
  - This way, we can put custom logic in them 

```python
@property
def name(self):
    return self._name

@name.setter
def name(self, value):
    self._name = value.upper()

@name.deleter
...
```

- We can use the `@classmethod` decorator to define a method which operates on the class, versus an *instance of the class*
  - The class will always be passed as the first argument (as opposed to the instance)
  - This is regardless of whether it is called on the class itself or an instance of the class, which is unlike undecorated methods

```python
@classmethod
def do_something(cls):
  ...
```

- Similarly, we can use the `@staticmethod` decorator to define a method which neither operates on the class nor an instance
  - No additional arguments passed

```python
@staticmethod
def do_something():
  ...
```

### Iterators
- Used when you want to iterate over items in an instance of a class
  - Notation: `i = iter(instance)`, `item = next(i)` or `for item in instance`
- Need to implement two dunder methods:
  - `__iter__` returns an instance of the iterator
    - If called on an iterator, should return `self`
  - `__next__` returns next item in iterator
    - `StopIteration` should be raised when no more items
- Alternative: `yield`
  - An alternative to implementing an iterator is to use yield in a loop
  - Python will manage the iterator for you

Examples using yield:
```python
class IterableClass:
  def __init__(self, data):
    self.data = data

  def __iter__(self):
    for item in self.data:
      yield item

def do_iteration():
  for i in range(10):
    yield i
```

yield from

#### Unpacking Iterators

Any iterable in Python can be unpacked into comma-separated variables, provided the number of variables matches the number of items in the iterable:

```python
def do_iteration(n):
  for i in range(n):
    yield i

var1, var2, var3 = do_iteration(5) # raises ValueError
var1, var2, var3 = do_iteration(3) # works!
```

Alternatively, you can use `*` to capture additional items into a list

```python
var1, *more = do_iteration(3) # works!
print(var1, more) # = 1, [2, 3]
```

#### Itertools

ToDo Potentially

### Special Classes

#### Enum

- Enums work how you'd expect

```python
from enum import Enum

class Weekday(Enum):
  MONDAY = 1
  TUESDAY = 2
  WEDNESDAY = 3
  ...
```

- You can also let Python auto assign enum values using `auto()`

```python
from enum import Enum, auto

class LightState(Enum):
  ON = auto() # will start at 1 by default
  OFF = auto()
```

- You can access the name/value of the enum member

```python
print(LightState.ON.name) # "on"
print(LightState.ON.value) # 1
```

- You can make enums that also operate as integers/strings by subclassing `IntEnum` or `StrEnum`
- You can make enums compatible with bitwise operations by subclassing `Flag`
  - Ex: `configure(SocketConfig.RESEND_ON_FAIL | SocketConfig.BIG_ENDIAN)`

#### Dataclasses

- Dataclasses are a simple way to define classes which primarily store data:

```python
from dataclasses import dataclass

@dataclass
class Product:
  name: str
  price: float
```

- Dunder methods like `__init__`, `__eq__`, etc are created automatically, to customize them you can:
  - Write your implementation of the method like you would for any other class, overriding the default
  - Use `field` to customize which fields (in our example: `name`, `price`) influence each dunder method

Example: using `field` to make dunder methods ignore id in comparisons (like `__eq__`)

```python
from dataclasses import dataclass, field

@dataclass
class Person:
  name: str
  age: int
  id: str = field(compare=False) 
```

Example: instantiating a dataclass

```python
person_1 = Person("Bill", 20, 1001)
```

## Namespaces, Scopes, and Variable Behavior
### Namespaces
- A namespace as a mapping from names to objects
  - Examples: built-in names (like `abs()`), global names in a module, local names in function invocation
- Attributes in a function can be thought of as a namespace (attributes being anything followed by a dot, ex: `z.real`)
- Namespaces have different lifetimes
  - A scope is region of a program where a namespace is directly accessible

### Scopes
- If you try to access a variable python will search the following scopes in order
  - Innermost scope (local names)
  - Scopes of any enclosing functions (non-local names)
  - Next-to-last scope containing the current modules global names
  - Outermost containing built-in names
- Assignments to names always go to innermost scope, except:
  - `global` - indicate variable lives in global scope
  - `nonlocal` - indicate variable lives in an enclosing scope


### Pass by Assignment
- Python is pass by assignment, not pass by reference or value
  - As stated above, namespaces in Python map names to objects
  - Everything, even integers, are objects in Python
- When we pass a variable to a function/method, we assign a new name to that object (let's say we name it `param`)
  - If the object is mutable, we can modify it within the function and it will be reflected outside
  - However, if we *assign* to `param`, that name now points to a new object
    - Since the object is now different, the change will not be reflected in the variable outside

Example: pass by assignment
```python
def func1(param):
  param.append(4)

var1 = [1, 2, 3]
func1(var1)
print(var1) # [1, 2, 3, 4]

def func2(param):
  param = [2, 3, 4]

var2 = [1, 2, 3]
func2(var2)
print(var2) # [1, 2, 3]
```

#### Keeping Track of Variable Identity
- Get the ID of the object currently referenced by a variable: `id(var)`
- Check if two variables reference the same object: `var1 is var2`
  - This compares IDs, unlike `var1 == var2` which invokes `__eq__`

#### Duplicating objects
- Sometimes, you need to copy an object, versus just assigning a new name to it
- There are two ways of copying
  - A shallow copy is a copy of an object that references the same nestled objects
  - A shallow copy is a copy of an object that recursively copies the nestled objects
  - An example of nestled objects are the items in a list
- Creating copies:

```python
import copy

var = [1, 2, 3]
shallow_copy = copy.copy(var)
deep_copy = copy.deepcopy(var)
```

- To enable copying custom classes, implement:
  - `__copy__` - return the shallow copy
  - `__deepcopy__(memo)` - call `copy.deepcopy(obj, memo)` for each nested object, return copy

### Naming Conventions
- Naming is mostly convention and nothing stops you from modifying a constant, for example
- Constants are declared `LIKE_THIS`
- 'Private' attributes/methods are declared `__like_this`
  - Private attributes are prefixed by the class name at runtime so if a subclass re-uses the name, they do not conflict (called name mangling)
- 'Protected' attributes/methods are declared `_like_this`
- 'Public' attributes/methods are declared `like_this`
- Names we don't care about are declared `_`, ex:

```python
for _ in range(5):
  print("Hello")
```

## Typing and Polymorphism
### Type System
- Python uses **duck typing**: where an object is considered compatible with a given type if it has all the methods and attributes that the type requires
  - Example: anything can be iterated over as long as it implements 2 required methods
- We can give arguments, return values, and attributes (and more!) **type hints** to improve readability and allow for some static type checking/better intellisense
  - These hints don't actually *do anything* at runtime

Example: adding type hints to a function's arguments & return value

```python
def add_two(a: int, b:int) -> int:
  return a + b
```

Example: adding type hints to an attribute

```python
def __init__(self):
  self.event_start: datetime | None = None
```

Example: defining a typed dictionary and using it to instantiate a variable

```python
from typing import TypedDict

class Movie(TypedDict):
  name: str
  year: int

movie: Movie = {
  "name": "Blade Runner",
  "year": 1982
}
```

### Inheritance
Example: deriving a class from a parent and overriding a method

```python
class Shape:
  def say_hi(self):
    return "Hi i'm a shape"

class Circle(Shape):
  def greet(self):
    return "Hi, i'm a circle"
```

Example: deriving a class from multiple parents

```python
class Shape:
  def say_name(self):
    return "Shape"

class Polygon(Shape):
  def say_name(self):
    return "Polygon"

class Opaque:
  def say_name(self):
    return "Opaque"

class Circle(Polygon, Opaque):
  def greet(self):
    return f"Hi, i'm {super().say_name()}" # Returns: Hi, i'm Polygon
```

#### Method Resolution Order: why 'polygon' was returned
- When `super().say_name()` is called, Python follows the MRO to locate the next class to implement `say_name` and calls it
  - It finds the next class to implement a method by returning a *Proxy Object* that we call the method on (details left out)
- The MRO is computed using C3 linearization, which is essentially DFS
  - Tiebreak: go left to right in list of parent classes
- To get the MRO, you can call `Class.mro()`
  - In our example: `Circle.mro()` returns:

```  
[
  <class '__main__.Circle'>, 
  <class '__main__.Polygon'>, 
  <class '__main__.Shape'>, 
  <class '__main__.Opaque'>, 
  <class 'object'>
]
```

- MRO and class initialization
  - Like other OOP languages, a subclass is expected to call a parent's constructor in it's constructor
  - Nuance: since method resolution follows a DFS ordering, a class which doesn't inherit from anything might still be required to call `super().__init__()`
    - For an example, think about what super constructor `Shape` would need to call in the above code snippet (following the MRO)
    - This obviously can get confusing, solution is to just not do fancy things with inheritance

### Abstract Base Classes (ABCs)
- ABCs are classes designed to be subclassed, but not directly instantiated
  - They must subclass `ABC`
- If a method must be overridden in a subclass, it must be decorated with `@abstractmethod`
  - If a class is instantiated with abstract methods that have not been overridden, an error is raised
  - An ABC can define attributes that must be implemented by decorating it's getter with `@abstractmethod`

Example: an ABC

```python
from abc import ABC, abstractmethod

class Shape(ABC):
  @property
  @abstractmethod
  def side_length(self) -> int:
    pass

  @abstractmethod
  def get_area(self) -> int:
    pass

class Square(Shape):
  def __init__(self, side_length) -> None:
    self._side_length = side_length

  def side_length(self) -> int:
    return self._side_length

  def get_area(self) -> int:
    return self._side_length ** 2
```

### Protocols
- Protocols let you use duck typing with Python's type hints system
  - You define a protocol with some methods/attributes with ellipsis (`...`) as their body
  - You use a protocol as a type hint
  - You gain static type checking that the object passed to the argument (for example) with the type hint has the methods/attributes defined in the protocol
- Unlike ABCs, protocols don't enforce a strict class hierarchy: as long as the object has the required methods/attributes it works

Example: a protocol

```python
from typing import Protocol, ClassVar

class MyProtocol(Protocol):
  instance_attr: str         # Instance attribute
  class_attr: ClassVar[int]  # Class attribute

  def method(self) -> None:
    ...

class ConformingClass:
  class_attr = 42

  def __init__(self):
    self.instance_attr = "Hello"
  
  def method(self) -> None:
    print(self.instance_attr)

my_class: MyProtocol = ConformingClass()
```

### Runtime Type Checks
- `type(obj) == Class`: evaluates to True if `obj` is an instance of `Class`
- `isinstance(obj, Class)`: returns True if `obj` is a instance of `Class` **or a subclass**

## Errors/Exceptions
- There are two types of error: syntax errors and exceptions

## External Data Structure Libraries
### SortedContainers
- SortedContainers contains SortedList, SortedSet, and SortedDict classes.
  - Each supports all methods of its non-sorted counterpart
  - Each are sorted by comparing the item (or for SortedDict, by key)

#### Additional SortedSet methods
- `SortedSet.index(item)`: index of item in set, or -1

#### Additional SortedDict methods
- `SortedDict.index(item)`: index of item in dict, or -1
- `SortedDict.peekitem(index)`: return item at index
- `SortedDict.popitem(index)`: **remove** & return item at index

[Additional Sorted Container Documentation](https://grantjenks.com/docs/sortedcontainers/)

## Data Display
### Matplotlib

- Create plot objects using `subplots()`:

```python
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
```

- Create plot & add data:

```python
# Scatter
x = [1, 2, 3, 4, 5]
y = [2, 3, 4, 5, 6]
ax.scatter(x, y)

# Bar
categories = ["A", "B", "C", "D"]
values = [10, 20, 15, 30]
ax.bar(categories, values)

# Histogram
data = [1, 2, 2, 3, 3, 3, 4, 4, 5, 6, 7]
bins = 5
ax.hist(data, bins=bins)
```

- Customize, save as file, close object:

```python
ax.set_title("Chart Title")
ax.set_xlabel("X Label")
ax.set_ylabel("Y Label")

fig.savefig("chart.png")
plt.close(fig)
```

[Additional Matplotlib Documentation](https://matplotlib.org/stable/users/index)

## File I/O
- Use `open(filename, mode)` to open files
- Can access files, then manually close:

```python
file = open("test.txt", "r")

file.read(3) # read 3 bytes
file.readline() # read one line

file.seek(0) # "rewind" file to start
file.read() # read till end

file.close()
```

- Or, can auto-close using a context manager:

```python
with open("test.txt", "w") as file:
  file.write("hello world")
```

### File access modes

| Mode | Description                  |
|------|------------------------------|
| r    | Read only, file must exist   |
| w    | Write or create then write   |
| a    | Append or create then write  |
| b    | Binary mode, used like: `wb` |
| +    | Read + write                 |

### CSV

- Read from a CSV:

```python
import csv

with open("test.csv") as file:
  reader = csv.reader(file)
  for line in reader:
    print(", ".join(line))
```

- Write to a CSV:

```python
write_test = [
  ["Name", "Age"],
  ["Max", 20],
  ["Bill", 21]
]

with open("test.csv", "w", newline="") as file:
  writer = csv.writer(file)

  # Write rows one at a time
  for line in write_test:
    writer.writerow(line) # line can be any iterable

  # Write rows all at once
  writer.writerows(write_test)
```

- Read/Write to a CSV from a dict:

```python
test_dict = [{"Name": "Max", "Age": 20}, {"Name": "Bill", "Age": 21}]

# Do write
with open("test_dict.csv", "w", newline="") as file:
  writer = csv.DictWriter(file, fieldnames=test_dict[0].keys())
  writer.writeheader()
  writer.writerows(test_dict)

# Do read
with open("test_dict.csv") as file:
  reader = csv.DictReader(file)
  for line in reader:
    print(line) # = {"Name": "Max", "Age": 20}
```

### JSON

- Load JSON:

```python
import json

obj_1 = json.load(filepointer) # from a file
obj_2 = json.loads(string) # from a string
```

- 'Dump' JSON:

```python
json.dump(obj_1, filepointer) # to a file
print(json.dumps(obj_1)) # to a string
```

- Interact with JSON objects as you would Python dicts/lists

### HTML/BeautifulSoup
A library for pulling data out of HTML (and XML documents).

Parsing a document:

```python
from bs4 import BeautifulSoup

# Can parse from a file pointer or a string
soup = BeautifulSoup(fp)
soup = BeautifulSoup("<html>data</html>")
```

Searching a document tree:

```python
# Find all <title> tags 
soup.find_all("title") # find_all: returns all matches as list

# Like selector querying in JS, can call on a tag to return matching descendants
body = soup.find("body") # find: returns first match vs all
body.find_all("p")

# Finds descendants where HTML attributes match the specified keyword arguments
body.find_all("div", class_="example") # need underscore b/c class reserved keyword in python
body.find_all(id="example") # don't need to specify tag name
body.find_all("div", {"data-id": "123"})

# Can pass a regex to argument that takes a string
pattern = re.compile(r'^example-\d+')
matched_divs = soup.find_all("div", class_=pattern)

# Find only direct descendants (equivalent to body > p)
body.find_all("p", recursive=False)

# Get a tag's children
body.contents # list of children
body.children # iterator over children

# Other ways to navigate from an tag
body.parent # direct parent
body.parents # iterator of parents up the tree
body.next_sibling # next sibling (also: .previous_sibling)
body.next_siblings # iterator of next siblings
```

Querying tags:

```python
div = soup.find("div")

div.name # = "div"
div.gettext() # Returns text content under tag
"id" in div # True if attribute exists on tag
div["id"] # Get attribute
```

## Parallelism
### Asyncio
- Coopererative multitasking system, good for I/O bound tasks.
- Coopererative means will only switch between tasks when the current task explicity gives up control

#### Task and Coroutine Management

- Run a coroutine asyncronously using a `Task`:

```python
# Create a task to run the given coroutine asynchronously
task = asyncio.create_task(some_coroutine())

# Do other stuff

try:
  print(await task) # Wait for task to complete and print result
except:
  print("Error!") # If error in coroutine, exception will be raised here

task.cancel() # Or, we could cancel the task
```

- Managing multiple coroutines and `Tasks`:

```python
# Iterate over tasks as they complete
#   You can also pass coroutines in the list, which will get automatically converted to tasks
for task in asyncio.as_completed(tasks):
  print(await task) # Get result of task

# Return result of all tasks after they complete
#   Like as_completed, you can also pass in coroutines
#   return_exceptions=True returns exceptions from tasks in the result list versus being raised
results = await asyncio.gather(*tasks, return_exceptions=True)
print(results)
```

- Adding timeouts:

```python
# If the code inside the block takes more than 5s, a asyncio.TimeoutError is raised
async with asyncio.timeout(5):
  await some_coro()
```

#### Run event loop
Create & run event loop (where `main` is top-level coroutine)

```python
asyncio.run(main())
```

#### Syncronousization Tools

- Locks:

```python
lock = asyncio.Lock()

async with lock:
  # Critical section - only one coroutine can access at a time
```

- Semaphores:

```python
semaphore = asyncio.Semaphore(2)

async with semaphore:
  # Limited access section - 2 coroutines can access at a time
```

- Asyncronous queue (good for producer/consumer pattern):

```python
queue = asyncio.Queue()

await queue.put(item)
item = await queue.get()
```

### Threading
- Preemptive multitasking system, good for running I/O bound tasks without needing to explicity indicate when it can switch between them
- Only one thread can run at a time, due to CPython's Global Interpreter Lock (GIL)

```python
import threading

def worker(name):
    print(f"Thread {name} is running")

thread = threading.Thread(target=worker, args=("web server", ))
thread.start() # Start thread
thread.join() # Wait for the thread to finish
```

- Other threading features to know:
  - Subclassing `Thread`
  - Thread pools
  - Syncronization primitives

### Multiprocessing
- True parallelism system which spawns multiple processes with independent memory spaces, bypassing the GIL
- Since threads can run concurrently, great for CPU intensive tasks


```python
import multiprocessing

def worker(name):
    print(f"Process {name} is running")

thread = multiprocessing.Process(target=worker, args=("web server", ))
thread.start() # Start process
thread.join() # Wait for the process to finish
```

- Other processing features to know:
  - **`Queue` and `Manager` for inter-process communication**
  - Subclassing `Process`
  - Process pools
  - Syncronization primitives

## Networking
### Making Requests
#### Synchronous API - requests
Requests is a simple synchronous library for making HTTP requests.

- Making a request:

```python
import requests

# replace 'get' with the HTTP verb (post, patch, etc)
r = requests.get("https://api.github.com/events")
```

- Adding data to a request:

```python
headers = {"user-agent": "my-app/0.0.1"} # HTTP Headers
data = {"key": "value"} # Request body
params = {"key1": "value1"} # URL params, etc: /get?key1=value1

r = requests.post("https://myapi.com/endpoint", headers=headers, data=data, params=params)
```

- Using the response:

```python
r.status_code # HTTP status code

r.raise_for_status() # Will raise relevant error if response != OK

r.headers # Response headers

r.content # Response body as raw binary
r.text # Response body as text
r.json() # Response body, parsed as JSON
```

#### Asynchronous API - aiohttp

- Create a client session (per application, usually):

```python
import aiohttp
import asyncio

async def main():
  async with aiohttp.ClientSession() as session:
    # requests here

asyncio.run(main())
```

- Or, can make session without a context manager:

```python
async def main():
  session = aiohttp.ClientSession()
  # requests here
  await session.close()
```

- Then make a request:

```python
data = {"key": "value"} # Request body
params = {"key1": "value1"} # URL params, etc: /get?key1=value1

async with session.get('http://httpbin.org/get', json=data, params=params) as resp:
  print(resp.status) # HTTP status code
  print(await resp.text()) # Response body as text
  print(await resp.json()) # Response body parsed as JSON
```

### FastAPI and WSGI/ASGI
Probably won't be in interviews.

## Modules, Packages, and Package Management
- A Python **module** is a file containing python code
  - You can import any module into another using an `import modulename` statement
- A **package** is a collection of python modules organized into directories
  - The existance of a `__init__.py` file denotes a directory as a package
- You can install external packages (dependencies) with **pip**
  - Ex: `pip install matplotlib`
- You can isolate dependencies for different projects using virtual enviroments:
  - To make a venv, run `python -m venv myenv` in your project's folder
  - To activate a venv (do before using pip), run: `source myenv/bin/activate`
  - To deactivate, run: `deactivate`
- To manage dependencies systematically generate a `requirements.txt`
  - To generate this: `pip freeze > requirements.txt`
  - To install dependencies from it: `pip install -r requirements.txt`
  - Then, `.gitignore` the venv folder

## Testing
### Testing with `unittest`
- Test cases are written as methods in a class than subclasses `unittest.TestCase`
- Assertions: available as instance methods
  - Always put expected value before actual
  - `self.assertEqual(a, b)`
  - `self.assertTrue(a)`
  - `self.assertRaises(exception, func, *args, **kwargs)`
  - `with self.assertRaises(exception)`: put code which should error in context manager block
  - `self.assertIn(a, b)`: assert `a in b`
- Setup & teardown
  - `setUp()`: code that runs before every test method
  - `tearDown()`: code that runs after every test method

Example: some unittest test cases

```python
import unittest

class TestMathOperations(unittest.TestCase):
  def setUp(self):
    print("Setting up resources")

  def tearDown(self):
    print("Cleaning up resources")

  def test_addition(self):
    self.assertEqual(1 + 1, 2)

  def test_subtraction(self):
    self.assertEqual(5 - 3, 2)

  def test_divide_by_zero(self):
    with self.assertRaises(ZeroDivisionError):
      1 / 0

if __name__ == "__main__":
  unittest.main()
```

### Static analysis with `mypy`
Probably won’t be in interviews.

## Documentation

## Interpreter details
[Here are really good notes](https://github.com/python/cpython/blob/main/InternalDocs/README.md)

## Decorators
Probably won’t be in interviews.