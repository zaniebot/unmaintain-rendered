```yaml
number: 403
title: False positive in invalid-argument-type
type: issue
state: closed
author: dleifeld
labels: []
assignees: []
created_at: 2025-05-15T08:40:42Z
updated_at: 2025-05-15T08:46:47Z
url: https://github.com/astral-sh/ty/issues/403
synced_at: 2026-01-10T02:34:09Z
```

# False positive in invalid-argument-type

---

_Issue opened by @dleifeld on 2025-05-15 08:40_

### Summary

Given this code 
````Python
from pydantic import BaseModel


class User(BaseModel):
    id: int
    name: str | None = None


def print_user(user: User) -> None:
    print(f"User ID: {user.id}")

    # According to mypy and Pylance this is correct
    # Only ty complains with this
    if user.name:
        print_name(user.name)
    if user.name is not None:
        print_name(user.name)

    # This is not correct
    print_name(user.name)


def print_name(name: str) -> None:
    print(f"Name: {name}")
````

I get the very helpful output three times:
````
error[invalid-argument-type]: Argument to function `print_name` is incorrect
  --> test.py:14:20
   |
12 |     # According to mypy and Pylance this is correct
13 |     if user.name:
14 |         print_name(user.name)
   |                    ^^^^^^^^^ Expected `str`, found `str | None`
15 |     if user.name is not None:
16 |         print_name(user.name)
   |
info: Function defined here
  --> test.py:22:5
   |
22 | def print_name(name: str) -> None:
   |     ^^^^^^^^^^ --------- Parameter declared here
23 |     print(f"Name: {name}")
   |
info: rule `invalid-argument-type` is enabled by default
````

I would expect it only once.
Given that this is still is alpha, I am impressed by the speed and quality of the errors.

### Version

ty 0.0.1-alpha.2

---

_Comment by @MichaReiser on 2025-05-15 08:46_

Thanks for the clear report. This is another instance of https://github.com/astral-sh/ty/issues/164

---

_Closed by @MichaReiser on 2025-05-15 08:46_

---
