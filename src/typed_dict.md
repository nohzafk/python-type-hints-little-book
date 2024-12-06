# typed-dict

Python's `TypedDict` is a more structured way to define dictionary types with specific keys and value types.

**1. Understanding TypedDict**

`TypedDict` allows you to define a dictionary type with a fixed set of keys, where each key has a specific value type. Think of it as a "schema" for your dictionaries.

**Basic Structure:**

```python
from typing import TypedDict

# Basic TypedDict definition
class UserProfile(TypedDict):
    name: str
    age: int
    email: str
```

Let's visualize the structure:

```goat
 UserProfile TypedDict
 +-------------------+
 |     name: str     |
 |     age: int      |
 |    email: str     |
 +-------------------+
       implements
          ↓
 {
   "name": "John",
   "age": 30,
   "email": "j@ex.com"
 }
```

**2. Advanced TypedDict Features**

```python
from typing import TypedDict, NotRequired

# Option 1: Using NotRequired (Python 3.11+)
class ProjectSettings(TypedDict):
    name: str  # Required
    version: NotRequired[str]  # Optional
    debug: NotRequired[bool]  # Optional

# Option 2: Inheritance approach (for older Python versions)
class RequiredSettings(TypedDict):
    name: str  # Required

class ProjectSettings(RequiredSettings, total=False):
    version: str  # Optional
    debug: bool  # Optional
```

Let's create a diagram to show the difference:

```goat
total=True (default)        total=False
+------------------+       +------------------+
|  name    [req]   |      |  name    [opt]   |
|  version [req]   |      |  version [opt]   |
|  debug   [req]   |      |  debug   [opt]   |
+------------------+       +------------------+

Using NotRequired
+------------------+
|  name    [req]   |
|  version [opt]   |
|  debug   [opt]   |
+------------------+
```

The key points:

- total=False makes ALL fields optional
- For mixed required/optional fields, use either:
  - NotRequired (Python 3.11+)
  - Inheritance approach (older Python versions)

**3. Practical Examples**

```python
from typing import TypedDict, List, Union

# Nested TypedDict structures
class Address(TypedDict):
    street: str
    city: str
    zip_code: str

class Company(TypedDict):
    name: str
    industry: str
    address: Address

# Usage with lists and unions
class ProjectMember(TypedDict):
    id: int
    role: str
    access_levels: List[str]
    metadata: Union[dict, None]

# Example implementation
def create_company(data: Company) -> Company:
    return {
        "name": data["name"],
        "industry": data["industry"],
        "address": data["address"]
    }
```

**4. Type Checking Behavior**

```python
from typing import TypedDict

# Basic TypedDict definition
class UserProfile(TypedDict):
    name: str
    age: int
    email: str

# Type checker will catch these errors:
user: UserProfile = {
    "name": "John",
    "age": "30",  # Type error: Expected int, got str
    "email": "john@example.com"
}

# Missing required fields:
incomplete: UserProfile = {
    "name": "John"  # Type error: Missing required fields
}

# Extra fields:
extra: UserProfile = {
    "name": "John",
    "age": 30,
    "email": "john@example.com",
    "extra": "field"  # Type error: Extra field not allowed
}
```

**5. Best Practices and Tips**

1. **Using `total` Parameter:**
```python
# All fields required (default)
class Config1(TypedDict):
    debug: bool
    cache: bool

# All fields optional
class Config2(TypedDict, total=False):
    debug: bool
    cache: bool

# Mixed required/optional (Python 3.11+)
class Config3(TypedDict):
    debug: Required[bool]  # Required
    cache: NotRequired[bool]  # Optional
```

2. **Documentation with TypedDict:**
```python
class APIResponse(TypedDict):
    """API Response structure for user data.

    Fields:
        status: HTTP status code
        data: Response payload
        message: Optional status message
    """
    status: int
    data: dict
    message: NotRequired[str]
```

**6. Common Pitfalls and Solutions**

```python
from typing import TypedDict, Union, Any

# ❌ Avoid using Any when possible
class BadConfig(TypedDict):
    settings: Any  # Too permissive

# ✅ Better approach
class GoodConfig(TypedDict):
    settings: Union[str, int, bool]  # Specific types

# ❌ Avoid nested plain dicts
class BadNesting(TypedDict):
    data: dict  # Too generic

# ✅ Better approach
class DataStructure(TypedDict):
    value: str
    type: str

class GoodNesting(TypedDict):
    data: DataStructure  # Type-safe nesting
```

**7. Advanced Use Cases**

1. **With Enums:**
```python
from enum import Enum
from typing import TypedDict

class UserRole(Enum):
    ADMIN = "admin"
    USER = "user"

class UserWithRole(TypedDict):
    name: str
    role: UserRole  # Using enum for type-safe roles
```

2. **With Literal Types:**
```python
from typing import TypedDict, Literal

class ThemeConfig(TypedDict):
    mode: Literal["light", "dark"]
    accent: Literal["blue", "green", "red"]
```
