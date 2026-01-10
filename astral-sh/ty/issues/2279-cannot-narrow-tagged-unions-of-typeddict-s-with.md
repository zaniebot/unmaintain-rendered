```yaml
number: 2279
title: "Cannot narrow tagged unions of `TypedDict`s with match statements"
type: issue
state: closed
author: felixgwilliams
labels:
  - narrowing
  - typeddict
assignees: []
created_at: 2025-12-30T15:46:46Z
updated_at: 2025-12-30T16:29:10Z
url: https://github.com/astral-sh/ty/issues/2279
synced_at: 2026-01-10T01:56:41Z
```

# Cannot narrow tagged unions of `TypedDict`s with match statements

---

_Issue opened by @felixgwilliams on 2025-12-30 15:46_

### Summary

The [mdtests](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/typed_dict.md#narrowing-tagged-unions-of-typeddicts) show that type narrowing works with tagged unions when the narrowing is done via an if statement. However, it does not work when a match statement is used.

The code block below gives an example. Mypy and Pyright/Pylance both reveal the types as `Foo` and `Bar`, but ty gives `Foo | Bar`.

```python
from __future__ import annotations

from typing import Literal, TypedDict, reveal_type


class Foo(TypedDict):
    tag: Literal["foo"]


class Bar(TypedDict):
    tag: Literal[42]

# if statement example omitted

def match_statements(u: Foo | Bar):
    match u["tag"]:
        case "foo":
            reveal_type(u) # ty gives: `Foo | Bar`
        case 42:
            reveal_type(u)
        case _:
            reveal_type(u)

```

Playground link: https://play.ty.dev/db75a410-f9cc-4004-b053-b89ded7adf1d

### Version

0.0.8

---

_Label `narrowing` added by @AlexWaygood on 2025-12-30 15:47_

---

_Label `typeddict` added by @AlexWaygood on 2025-12-30 15:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-30 16:09_

---

_Closed by @charliermarsh on 2025-12-30 16:29_

---
