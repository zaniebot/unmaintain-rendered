```yaml
number: 657
title: Type checker incorrectly rejects Pydantic field aliases in constructors
type: issue
state: closed
author: davenpi
labels: []
assignees: []
created_at: 2025-06-14T14:35:44Z
updated_at: 2025-09-03T13:00:47Z
url: https://github.com/astral-sh/ty/issues/657
synced_at: 2026-01-10T02:06:24Z
```

# Type checker incorrectly rejects Pydantic field aliases in constructors

---

_Issue opened by @davenpi on 2025-06-14 14:35_

## Summary

`ty` reports `unknown-argument` errors when using Pydantic field aliases in model constructors, even when `validate_by_alias=True` is configured. This is valid Pydantic code that runs correctly at runtime.

## Minimal reproducible example

```python
# alias.py
from pydantic import BaseModel, ConfigDict, Field

class MyModel(BaseModel):
    model_config = ConfigDict(extra="allow", validate_by_alias=True, validate_by_name=True)

    full_name: str = Field(alias="fullName")
    age: int

# This works fine
m1 = MyModel(full_name="John David", age=30)

# This also works at runtime but ty reports an error
m2 = MyModel(fullName="John David", age=30)  # <-- ty error here

print(m1)  # name='John David' age=30
print(m2)  # name='John David' age=30
```

## Run command (default settings)

```bash
uv run ty check alias.py
# or
ty check alias.py
```

## Expected behavior 

No type checking errors. Both constructor forms should be accepted when `validate_by_alias=True` is configured.

## Actual Behavior

```
error[unknown-argument]: Argument fullName does not match any known parameter
```

## Environment
- ty version: 0.0.1-alpha.10 (15bae14c2 2025-06-13)
- Pydantic version: 2.11.6
- Python version: 3.12

## Additional Context

This pattern is common in Pydantic for handling JSON APIs that use camelCase while maintaining Pythonic snake_case field names internally.

### Version

ty 0.0.1-alpha.10 (15bae14c2 2025-06-13)

---

_Comment by @carljm on 2025-06-17 02:09_

This should be fixed by the "support for dataclass fields" item of https://github.com/astral-sh/ty/issues/111 -- I'll close it as a duplicate, since it's a known issue, but this is a useful example, thank you!

---

_Closed by @carljm on 2025-06-17 02:09_

---

_Comment by @maver1ck on 2025-09-02 18:48_

Hi @carljm 
support for dataclass fields is checked as done, however this bug persists.
Any idea why ?

---

_Comment by @sharkdp on 2025-09-02 19:18_

I think this is now (still) blocked on https://github.com/astral-sh/ty/issues/1068

---

_Reopened by @carljm on 2025-09-03 00:06_

---

_Closed by @carljm on 2025-09-03 00:06_

---

_Comment by @PrettyWood on 2025-09-03 12:52_

I'll work on it on Sunday

---

_Comment by @sharkdp on 2025-09-03 13:00_

> I'll work on it on Sunday

That sounds great. Please note that the "(still)" in my comment just referred to the fact that it has always been blocked by missing `field_specifiers` support, despite status updates in #111. I didn't mean to imply any sort of "delay" or urgency.

---
