# final

**1. Basic Understanding of Final**

The `Final` type hint, introduced in Python 3.8 (PEP 591), indicates that a variable or attribute should only be assigned once and not be reassigned after its initialization. It's similar to the `final` keyword in languages like Java.

**2. Core Concepts**

Let's look at the different ways to use `Final`:

```python
from typing import Final

# Simple Final variable
COUNT: Final = 100

# Final with type specification
MAX_SIZE: Final[int] = 1000

# Final in class attributes
class Config:
    DEBUG: Final[bool] = False
    
    def __init__(self):
        self.API_KEY: Final[str] = "abc123"  # Instance attribute
```

**3. Visual Representation of Final Assignment Flow**

```goat
    Initial Assignment                    Reassignment Attempt
    
    COUNT: Final = 100                   COUNT = 200
      |                                     |
      v                                     v
    [Memory Location]      →→→      [Type Checker Error!]
         "100"                    "Cannot assign to final name"
```

**4. Advanced Usage Patterns**

Let's explore more complex scenarios:

```python
from typing import Final, List, Dict

# With collections
ALLOWED_STATES: Final[List[str]] = ["PENDING", "ACTIVE", "CLOSED"]

# With nested structures
CONFIG: Final[Dict[str, int]] = {
    "timeout": 30,
    "retries": 3
}

# With class type annotations
class User:
    DEFAULT_ROLE: Final[str] = "guest"
    
    def __init__(self, name: str):
        self.name: Final[str] = name
```

**5. Key Insights and Behavior**

1. **Content Mutability**
```python
# The container is final, but content can be mutable
ITEMS: Final[List[int]] = [1, 2, 3]
ITEMS.append(4)  # This is allowed!
ITEMS = [4, 5, 6]  # This raises an error
```

2. **Type Inheritance**
```python
class Parent:
    x: Final[int] = 1

class Child(Parent):
    x = 2  # Error: cannot override final attribute
```

**6. Common Use Cases**

1. **Constants Definition**
```python
class DatabaseConfig:
    MAX_CONNECTIONS: Final[int] = 100
    TIMEOUT: Final[float] = 30.0
```

2. **Immutable Configuration**
```python
class APISettings:
    BASE_URL: Final[str] = "https://api.example.com"
    API_VERSION: Final[int] = 1
```

**7. Best Practices and Pitfalls**

Let's visualize the difference between Final variable and Final content:

```goat
Final Variable (Immutable Reference)
                                        ╔════════════╗
CONST: Final[list] = [1,2,3]  ═══════►  ║ [1,2,3]    ║
                                        ╚════════════╝
                                             ▲
                                             │ (can modify content)
                                        append(4)
                                             │
                                        [1,2,3,4]

Final Content (Immutable Content)
                                        ╔═════════════╗
from typing import FrozenSet            ║             ║
CONST: Final[FrozenSet] = frozenset()   ║  {1,2,3}    ║
                                        ║ (immutable) ║
                                        ╚═════════════╝
```

Let me create a diagram showing the type checking process for Final variables:

**8. Important Notes:**

1. **Type Checker Only**
   - `Final` is enforced by type checkers (like mypy)
   - No runtime enforcement by Python itself

2. **Initialization Requirements**
   - Must be initialized immediately or in `__init__`
   - Cannot be assigned in other methods

3. **Inheritance Behavior**
   - Cannot override Final attributes in subclasses
   - Can inherit Final attributes from parent classes

**When to Use Final:**

1. When defining true constants that should never change
2. For configuration values that should be immutable
3. To prevent accidental override of important attributes
4. To make code intentions clear to other developers

