# kwargs

## Two Correct Ways to Type kwargs

### 1. Value Type Annotation
```python
def foo(**kwargs: int | str):
    ...
```
This is the standard way to type kwargs when you want to specify what types the values can be. The `**kwargs` syntax automatically handles the dictionary structure, and you only need to specify the types of values.

Example usage:
```python
foo(a=1, b="hello")  # Valid
foo(x=1.0)  # Invalid - float not allowed
```

### 2. TypedDict with Unpack (Python 3.12+)
```python
from typing import TypedDict, Unpack

class Movie(TypedDict):
    name: str
    year: int

def foo(**kwargs: Unpack[Movie]):
    ...
```
Use this when you need to specify exact structure of the kwargs with specific field names and their types.

Example usage:
```python
foo(name="Life of Brian", year=1979)  # Valid
foo(name="Life of Brian")  # Invalid - missing year
foo(name="Life of Brian", year="1979")  # Invalid - year must be int
```


Mark all keywords as optional with `total=False` 

```python
class Kw(TypedDict, total=False):
    key1: int
    key2: str
```

Mark specific keywords as optional with `typing.NotRequired`
```python
class Kw(TypedDict):
    key1: int
    key2: NotRequired[str]
```


## Common Mistake to Avoid

```python
# INCORRECT:
def foo(**kwargs: dict[str, int | str]):
    ...
```
This is wrong because:
1. It tries to make each value a dictionary
2. The `**` syntax already provides the dictionary structure
3. Will cause type checker errors when used

## Test Results Example
```python
def foo1(**kwargs: dict[str, int | str]):  # Wrong
    ...
def foo2(**kwargs: int | str):  # Correct
    ...
def foo3(**kwargs: Unpack[Movie]):  # Correct
    ...

# Test calls
foo1(**{"name": "Life of Brian", "year": 1979})  # Error
foo2(**{"name": "Life of Brian", "year": 1979})  # OK
foo3(**{"name": "Life of Brian", "year": 1979})  # OK
```
