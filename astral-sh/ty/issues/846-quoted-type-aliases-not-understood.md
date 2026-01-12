```yaml
number: 846
title: Quoted type aliases not understood
type: issue
state: closed
author: sterliakov
labels: []
assignees: []
created_at: 2025-07-17T21:26:32Z
updated_at: 2025-11-20T00:21:15Z
url: https://github.com/astral-sh/ty/issues/846
synced_at: 2026-01-12T15:54:24Z
```

# Quoted type aliases not understood

---

_@sterliakov_

### Summary

Check the following code:

```python
from typing import TypeAlias

Foo: TypeAlias = "int | str"
Bar: TypeAlias = int | str

x: Foo = 0  # Fails
y: Bar = 0  # OK
```

Output of `uvx ty check a.py --output-format concise` (without any config files, in a fresh `mktemp -d` with the above snippet cat'd into `a.py`):

```
error[invalid-type-form] a.py:6:4: Variable of type `Literal["int | str"]` is not allowed in a type expression
Found 1 diagnostic
```

[playground link](https://play.ty.dev/3c4a53d0-9a1e-41ed-889c-196333bbe242)

As per [the spec](https://typing.python.org/en/latest/spec/aliases.html#typealias), quotes should be ignored in the RHS, it is not a literal definition:) 

This isn't a toy problem - quoted RHS is the most convenient way to define a recursive alias (at least prior to PEP695, but I'd personally use this on 3.12+ too) or make it dependent on `TYPE_CHECKING`-only imports.

```python
Foo: TypeAlias = "int | Iterable[Foo]"
```

### Version

0.0.1-alpha.14

---

_Comment by @sharkdp on 2025-07-18 06:49_

Thank you for reporting this. We do not support `typing.TypeAlias` yet, see https://github.com/astral-sh/ty/issues/544. Once we do, it will also (automatically) support stringified annotations.

Stringified annotations are already supported in other places where type expressions are expected: https://play.ty.dev/5dbc7ee3-7b68-4498-9424-224baad866a1
```py
type IntOrStr = "int | str"

def _(x: "int | str", y: IntOrStr):
    reveal_type(x)  # int | str
    reveal_type(y)  # int | str

    x = 1
    y = 1
```

---

_Closed by @sharkdp on 2025-07-18 06:49_

---

_Comment by @carljm on 2025-11-20 00:21_

Confirmed that this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
