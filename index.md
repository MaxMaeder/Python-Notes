---
layout: home
title: Notes
---

## Control Flow, Variables
while, else loops
walrus operator
swapping two variables
ternary
Structural Pattern Matching / switch statements
underscores in variable values


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

### Useful Builtin Functions

## Data Structures
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

my_list.clear()
my_list.index(3) # get first index of item equal to 3
my_list.count(3) # count items equal to 3
my_list.reverse() # reverse list

3 in my_list # inefficient, o(n) check
```

#### List Comprehension

#### List Slicing, Indexing

### Tuple
Python tuples are immutable, ordered collections of items, backed by a fixed-size array.

```python
my_tuple = (1, 2, 3)
another_tuple = (1, ) # if tuple one item, needs trailing comma

# Tuples support all methods from lists that don't involve mutation 
```

#### Named Tuple

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

my_dict |= {'e': 5} # merge another dictionary (Python 3.9+)
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
- A counter counts hashable objects passed to its constructor, then can be interacted with just like a dict

```python
from collections import Counter

c = Counter([1, 1, 2, 3, 4])
c # = Counter({1: 2, 2: 1, 3: 1, 4: 1})

c.most_common(2) # return 2 highest counts as (item, count) pairs
c.subtract(other_count) # subtract other_count
c.total() # return total count
```

### Deque (pronounced deck)

### Heap

### DateTime

### Fraction/Decimal

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

#### Unpacking Iterators

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
  - Private attributes are prefixed by the class name at runtime so if a subclass re-uses the name, they do not conflict
- 'Protected' attributes/methods are declared `_like_this`
- 'Public' attributes/methods are declared `like_this`

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

## Errors


## File I/O
### Path

### CSV

### JSON


## Networking
### Making Requests

### FastAPI

### WSGI/ASGI

## Parallelism
### Asyncio
https://stackoverflow.com/questions/42231161/asyncio-gather-vs-asyncio-wait-vs-asyncio-taskgroup

### Threading

### Parallelism

## Modules, Packages, and Package Management

## Testing
### Testing with `unittest`
- Always put expected value before actual

```python
import unittest

class TestStatisticalFunctions(unittest.TestCase):

  def test_average(self):
    self.assertEqual(40.0, average([20, 30, 70]))
    self.assertEqual(4.3, round(average([1, 5, 7]), 1))
    with self.assertRaises(ZeroDivisionError):
      average([])
    with self.assertRaises(TypeError):
      average(20, 30, 70)

if __name__ == "__main__":
  unittest.main()
```

## Documentation

## Interpreter details

## Decorators