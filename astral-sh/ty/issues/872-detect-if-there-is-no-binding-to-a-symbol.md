---
number: 872
title: "Detect if there is no binding to a symbol declared `Final`"
type: issue
state: open
author: sharkdp
labels:
  - typing semantics
assignees: []
created_at: 2025-07-22T14:31:54Z
updated_at: 2026-01-09T02:01:30Z
url: https://github.com/astral-sh/ty/issues/872
synced_at: 2026-01-10T01:48:23Z
---

# Detect if there is no binding to a symbol declared `Final`

---

_Issue opened by @sharkdp on 2025-07-22 14:31_

This should be an error: the constant is declared `Final`, but there is no binding:

```py
CONSTANT: Final[int]  # this should be an error
```

Note that we *do* allow separating the declaration and the binding of a `Final` symbol:

```py
CONSTANT: Final[int]  # this is allowed
CONSTANT = 1
```

Also, even if we would disallow this particular pattern (it's not explicitly required by the spec), `Final`-annotations without a right-hand side can appear in (data)class definitions, for example:

```py
class C:
    CONSTANT: Final[int]  # this is allowed

    def __init__(self):
        self.CONSTANT = 1
```

We have existing tests for this [here](https://github.com/astral-sh/ruff/blob/64e57800373354cbabf106a5e89f6abc19d67300/crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md?plain=1#L358-L394).

---

_Label `typing semantics` added by @sharkdp on 2025-07-22 14:31_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 02:01_

---
