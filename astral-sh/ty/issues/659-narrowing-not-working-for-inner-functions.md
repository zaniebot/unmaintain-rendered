```yaml
number: 659
title: Narrowing not working for inner functions
type: issue
state: closed
author: galah92
labels:
  - narrowing
assignees: []
created_at: 2025-06-15T07:02:50Z
updated_at: 2025-07-25T07:11:12Z
url: https://github.com/astral-sh/ty/issues/659
synced_at: 2026-01-10T02:06:24Z
```

# Narrowing not working for inner functions

---

_Issue opened by @galah92 on 2025-06-15 07:02_

### Summary

[Playground link](https://play.ty.dev/bc4e0448-5cd5-42cf-8bbd-e61ac1105328)

```python
from collections.abc import Callable

def foo(goo: Callable | None = None):
    if goo is not None:
        def inner():
            goo()
```

### Version

5e02d839d

---

_Comment by @mtshiba on 2025-06-16 05:34_

This is the intended behavior.

doc: https://github.com/astral-sh/ruff/blob/89d915a1e34144051815dfcbe60ec2cdeb29909e/crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md#narrowing-constraints-introduced-in-the-outer-scope

This kind of narrowing is only safe in eager nested scopes. Consider the following code, where narrowing is unsound:

```python
from collections.abc import Callable

def foo(goo: Callable[[], int] | None = None):
    if goo is not None:
        class Inner:  # eager scope
            goo()  # int
        def inner():  # lazy scope
            return goo()  # int?
    goo = None
    inner() # error, not int!
```

However, narrowing can be safely performed if it's certain that there is no reassignment. It's a TODO.

---

_Label `narrowing` added by @carljm on 2025-06-17 01:55_

---

_Closed by @MichaReiser on 2025-07-25 07:11_

---
