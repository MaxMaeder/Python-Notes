## Functions
- Functions can be declared within each other
- By convention functions are declared before they are used

### Function Arguments

### Useful Builtin Functions

## Data Structures

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
- So, methods are not `static` or `instance` like in other languages, nothing stops a user from calling them in one context or another
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

## Polymorphism

## Errors


## File I/O
### CSV

### JSON


## Networking
### Making Requests

### FastAPI

## Parallelism
### Asyncio

### Threading

### Parallelism

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
