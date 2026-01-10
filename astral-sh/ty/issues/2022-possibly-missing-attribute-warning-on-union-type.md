```yaml
number: 2022
title: possibly-missing-attribute warning on Union type narrowing based on Literal values
type: issue
state: closed
author: arthur-st
labels: []
assignees: []
created_at: 2025-12-17T17:14:54Z
updated_at: 2025-12-17T17:48:07Z
url: https://github.com/astral-sh/ty/issues/2022
synced_at: 2026-01-10T01:53:59Z
```

# possibly-missing-attribute warning on Union type narrowing based on Literal values

---

_Issue opened by @arthur-st on 2025-12-17 17:14_

### Summary

```python
from pydantic import BaseModel
from typing import Literal

class Foo(BaseModel):

    foo: str
    kind: Literal["foo"] = "foo"

class Bar(BaseModel):

    bar: str
    kind: Literal["bar"] = "bar"

FooBar = Foo | Bar

def baz(thing: FooBar):
    if thing.kind == "bar":
        print(thing.bar)
```

This code shows the following warning:

```
❯ ty check ty-test.py
warning[possibly-missing-attribute]: Attribute `bar` may be missing on object of type `Foo | Bar`
  --> ty-test.py:18:15
   |
16 | def baz(thing: FooBar):
17 |     if thing.kind == "bar":
18 |         print(thing.bar)
   |               ^^^^^^^^^
   |
info: rule `possibly-missing-attribute` is enabled by default
```

This warning is not correct, since `Bar` is the only type with the `kind="bar"` attribute.

### Version

ty 0.0.2 (Homebrew 2025-12-16)

---

_Comment by @arthur-st on 2025-12-17 17:34_

Related #2023 

---

_Comment by @AlexWaygood on 2025-12-17 17:48_

Thanks for the report. Please see #1479

---

_Closed by @AlexWaygood on 2025-12-17 17:48_

---
