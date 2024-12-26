---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Notes
---

## Functions
- Functions can be declared within each other
- By convention functions are declared before they are used

### Function Arguments

### Useful Builtin Functions

## Data Structures

### List

### Dict
#### DefaultDict
#### Counter

### Deque (pronounced deck)

### Heap

### DateTime

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

## Code Organization and Accessibility
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

### Naming Conventions
- Naming is mostly convention and nothing stops you from modifying a constant, for example
- Constants are declared `LIKE_THIS`
- 'Private' attributes/methods are declared `__like_this`
  - Private attributes are prefixed by the class name at runtime so if a subclass re-uses the name, they do not conflict
- 'Protected' attributes/methods are declared `_like_this`
- 'Public' attributes/methods are declared `like_this`

## Polymorphism, Inheritance, and Typing
### Typing
- Python uses **duck typing**: where an object is considered compatible with a given type if it has all the methods and attributes that the type requires
  - Example: anything can be iterated over as long as it implements 2 required methods
- We can give arguments, return values, and attributes **type hints** to improve readability and allow for some static type checking/better intellisense
  - These hints don't actually *do anything* at runtime

Example: adding type hints to a function's arguments & return value:
```python
def add_two(a: int, b:int) -> int:
  return a + b
```
Example: adding type hints to an attribute:
```python
def __init__(self):

```

#### Protocols

## Errors


## File I/O
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