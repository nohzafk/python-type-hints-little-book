# literalstring

**1. LiteralString vs Literal Type**

While they might sound similar, `LiteralString` and `Literal` serve different purposes:

`Literal` (which you're familiar with):
```python
from typing import Literal

# Restricts values to specific literals
status: Literal["success", "error"] = "success"
```

`LiteralString`:
- Introduced in Python 3.11 (PEP 675)
- Used to indicate that a string is "literal" - meaning it's known at compile time
- Helps prevent SQL injection and similar security vulnerabilities
- Cannot be constructed from user input or dynamic strings

Example usage:
```python
from typing import LiteralString

def unsafe_query(sql: str):
    # Accepts any string, potentially unsafe
    pass

def safe_query(sql: LiteralString):
    # Only accepts literal strings, safer
    pass

# These are valid LiteralString values:
safe_query("SELECT * FROM users")
TABLE = "users"
safe_query(f"SELECT * FROM {TABLE}")  # OK if TABLE is a constant

# These are NOT valid LiteralString values:
user_input = input("Enter table: ")
safe_query(f"SELECT * FROM {user_input}")  # Type error
safe_query(user_input)  # Type error
```
**High-Level Insights:**

1. **Security Implications:**
   - `LiteralString` is part of Python's type system security features
   - Helps prevent injection vulnerabilities by ensuring strings are known at compile time
   - Type checkers can verify that sensitive functions only receive safe strings

2. **Best Practices:**
   - Use `LiteralString` for security-sensitive string parameters
