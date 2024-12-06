# Callable

Python's Callable type hint is particularly useful when working with functions as arguments or return values.

**Core Concept:**
The Callable type hint specifies that an object can be called like a function. It's part of the typing module and has two main components: the argument types and the return type.

Let's break this down with clear examples and visualizations:

**1. Basic Syntax:**
```python
from typing import Callable

# General form:
# Callable[[ArgumentType1, ArgumentType2, ...], ReturnType]
```

Let's visualize the structure:
```goat
                    Callable
                       |
            [Input Types] -> Return Type
                |              |
         +------+------+       |
         |      |      |       |
     Type1   Type2   Type3    Type
```

**2. Common Usage Patterns:**

```python
from typing import Callable

# Function that takes no arguments and returns str
no_args: Callable[[], str]

# Function that takes int, str and returns bool
checker: Callable[[int, str], bool]

# Function that takes multiple arguments of same type
processor: Callable[[str, str, str], list[str]]
```

**3. Practical Examples:**

```python
from typing import Callable

# Example 1: Simple callback
def execute_with_callback(callback: Callable[[int], None]) -> None:
    result = 42
    callback(result)

# Example 2: Function that returns a function
def create_multiplier(factor: int) -> Callable[[int], int]:
    def multiplier(x: int) -> int:
        return x * factor
    return multiplier

# Example 3: Higher-order function
def apply_operation(numbers: list[int], 
                   operation: Callable[[int], int]) -> list[int]:
    return [operation(num) for num in numbers]
```

Let's create a visual representation of Example 3's flow:

```goat
   numbers [1,2,3]      operation(x)
        |                    |
        v                    v
    +-------+           +---------+
    |  [1]  |--------→  | x * 2   | ----→ [2]
    |  [2]  |--------→  |         | ----→ [4]
    |  [3]  |--------→  |         | ----→ [6]
    +-------+           +---------+
        |                    |
        v                    v
    Input List        Callable Function
```

**4. Advanced Usage:**

```python
from typing import Callable, TypeVar, ParamSpec

# Generic types for more flexible callable signatures
T = TypeVar('T')
P = ParamSpec('P')

# Generic callback type
def register_callback(callback: Callable[P, T]) -> Callable[P, T]:
    return callback

# Multiple callable parameters
def compose(f: Callable[[int], str],
           g: Callable[[str], bool]) -> Callable[[int], bool]:
    return lambda x: g(f(x))
```

**5. Common Patterns and Best Practices:**

```python
from typing import Callable

# ✅ Good Practices
def process_data(processor: Callable[[str], str]) -> str:
    return processor("data")

# ✅ Optional parameters with default values
# str() is itself a callable (function) that can convert an integer to a string
def with_default(callback: Callable[[int], str] = str) -> str:
    return callback(42)

# ❌ Avoid: Ambiguous callable types
def bad_example(func: Callable):  # Too generic!
    return func()
```

**6. Key Insights:**

1. **Type Safety**: Callable helps catch type-related errors at development time.
2. **Documentation**: It serves as self-documenting code for function signatures.
3. **IDE Support**: Modern IDEs can provide better autocomplete and error detection.
4. **Flexibility**: Can represent any callable object (functions, methods, lambdas).

**7. Common Gotchas:**

1. **Overloaded Functions**: May need `@overload` decorator for multiple signatures
2. **Method Types**: Instance methods need special handling due to 'self' parameter
3. **Variadic Arguments**: Use `...` for variable number of arguments

**Example of handling these cases:**
```python
from typing import Callable, overload

# Overloaded function
@overload
def process(func: Callable[[int], int]) -> int: ...
@overload
def process(func: Callable[[str], str]) -> str: ...

def process(func):
    return func(42)

# Variadic arguments
from typing import Any
handler: Callable[..., None]  # Any number of arguments
```
