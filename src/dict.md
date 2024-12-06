# dict

**Understanding Dict Type Hints**

Let's explore Python's dictionary type hints through a visual progression from basic to advanced usage.

```mermaid
flowchart LR
    A[Dict Type Hints] --> B[Basic Dict]
    A --> C[Nested Dict]
    A --> D[Union Types]
    A --> E[TypedDict]

    B --> B1["Dict[str, int]"]
    B --> B2["Dict[str, Any]"]

    C --> C1["Dict[str, Dict[str, int]]"]

    D --> D1["Dict[str, Union[int, str]]"]
    D --> D2["Dict[Union[str, int], str]"]

    E --> E1["Class-based TypedDict"]
    E --> E2["dict-syntax TypedDict"]
 ```

**1. Basic Dictionary Type Hints**

```python
from typing import Dict

# Basic dictionary with string keys and integer values
scores: Dict[str, int] = {"Alice": 95, "Bob": 87}

# Dictionary with Any type values
from typing import Any
flexible_dict: Dict[str, Any] = {
    "name": "John",
    "age": 30,
    "active": True
}
```

**2. Nested Dictionaries**

```python
# Nested dictionary type hint
user_data: Dict[str, Dict[str, str]] = {
    "user1": {"name": "Alice", "email": "alice@example.com"},
    "user2": {"name": "Bob", "email": "bob@example.com"}
}
```

**3. Union Types in Dictionaries**

```python
from typing import Union

# Dictionary with multiple possible value types
mixed_values: Dict[str, Union[int, str]] = {
    "age": 25,
    "name": "Alice",
    "id": "A123"  # Can be either string or int
}

# Dictionary with multiple possible key types
flexible_keys: Dict[Union[str, int], str] = {
    "name": "Alice",
    1: "first place",
    "email": "alice@example.com"
}
```

**4. TypedDict (Advanced Usage)**

```python
from typing import TypedDict

# Class-based syntax
class UserProfile(TypedDict):
    name: str
    age: int
    email: str
    is_active: bool

# Usage
user: UserProfile = {
    "name": "Alice",
    "age": 30,
    "email": "alice@example.com",
    "is_active": True
}

# Dict-syntax (alternative way)
from typing_extensions import TypedDict

User = TypedDict('User', {
    'name': str,
    'age': int,
    'email': str
})
```

**5. Optional Keys in TypedDict**

`total=False` means all keys are optional

```python
class OptionalUserProfile(TypedDict, total=False):
    name: str
    age: int
    email: str
    phone: str
```

**Key Insights and Best Practices:**

1. **Type Specificity**
   - Be as specific as possible with types
   - Use `Any` only when absolutely necessary

2. **TypedDict vs Regular Dict**
   - Use `TypedDict` when you need to enforce specific key-value structure
   - Use regular `Dict` for more flexible dictionary structures

3. **Performance Considerations**
   - Type hints are removed at runtime
   - No performance impact during execution
   - Helpful for development and static type checking

**Common Pitfalls to Avoid:**

```python
# ❌ Don't do this (too broad)
data: Dict = {"key": "value"}  # Missing type parameters

# ✅ Do this instead
data: Dict[str, str] = {"key": "value"}

# ❌ Don't use Any unless necessary
flexible: Dict[str, Any] = {"name": "John"}

# ✅ Be specific when possible
user_data: Dict[str, Union[str, int, bool]] = {
    "name": "John",
    "age": 30,
    "active": True
}
```

**Advanced Tips:**

1. **Using with Protocol for Duck Typing**
```python
from typing import Protocol, Dict

class Serializable(Protocol):
    def to_dict(self) -> Dict[str, Any]: ...

def save_data(obj: Serializable) -> None:
    data = obj.to_dict()
    # Save data...
```

2. **Generic Dictionaries**
```python
from typing import TypeVar, Dict

K = TypeVar('K')
V = TypeVar('V')

def get_first_item(d: Dict[K, V]) -> V:
    return next(iter(d.values()))
```
