```yaml
number: 671
title: dataclass causes incorrect invalid-argument-type error
type: issue
state: closed
author: lexicalunit
labels: []
assignees: []
created_at: 2025-06-17T19:11:17Z
updated_at: 2025-06-17T19:21:32Z
url: https://github.com/astral-sh/ty/issues/671
synced_at: 2026-01-12T15:54:23Z
```

# dataclass causes incorrect invalid-argument-type error

---

_@lexicalunit_

### Summary

```python
import random
from dataclasses import dataclass


@dataclass
class Foo:
    a: str | None = None


def show_a(a: str) -> str:
    return f"foo: {a}"


def get_foo() -> Foo:
    if random.choice([True, False]):
        return Foo(a="Hello")
    return Foo()


foo = get_foo()
result = show_a(foo.a) if foo.a else None
print(result)
```

```text
error[invalid-argument-type]: Argument to function `show_a` is incorrect
  --> foo.py:22:17
   |
20 | foo = get_foo()
21 |
22 | result = show_a(foo.a) if foo.a else None
   |                 ^^^^^ Expected `str`, found `str | None`
23 |
24 | print(result)
   |
info: Function defined here
  --> foo.py:10:5
   |
10 | def show_a(a: str) -> str:
   |     ^^^^^^ ------ Parameter declared here
11 |     return f"foo: {a}"
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.10 (15bae14c2 2025-06-13)

---

_Renamed from "keyword only dataclass causes incorrect invalid-argument-type error" to "dataclass causes incorrect invalid-argument-type error" by @lexicalunit on 2025-06-17 19:11_

---

_Comment by @lexicalunit on 2025-06-17 19:12_

At first I thought it was related to `@dataclass(kw_only=True)` vs `@dataclass` but I'm actually getting the error either way now 🤔 

---

_Comment by @carljm on 2025-06-17 19:21_

This looks like #164 (not related to dataclasses at all), which coincidentally was just fixed earlier today. Fix is not released yet, but you can already see that your code example works in the playground: https://play.ty.dev/37c2507e-1489-4824-8a50-18819ad35343

---

_Closed by @carljm on 2025-06-17 19:21_

---
