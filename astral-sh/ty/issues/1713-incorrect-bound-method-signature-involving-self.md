```yaml
number: 1713
title: "Incorrect bound method signature involving `Self`"
type: issue
state: closed
author: ibraheemdev
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-01T23:44:43Z
updated_at: 2025-12-02T18:15:11Z
url: https://github.com/astral-sh/ty/issues/1713
synced_at: 2026-01-10T01:56:40Z
```

# Incorrect bound method signature involving `Self`

---

_Issue opened by @ibraheemdev on 2025-12-01 23:44_

### Summary

```py
from typing import Self, reveal_type

class Foo[T]:
    def foo(self) -> T:
        raise NotImplementedError

class Bar:
    def _(self, x: Foo[Self]):
        reveal_type(x.foo)  # revealed: bound method Foo[Self@_].foo() -> Foo[Self@_]

def f[T: Bar](x: Foo[T]):
    reveal_type(x.foo)  # revealed: bound method Foo[T@f].foo() -> T@f
```

The return type of the first is incorrect.

Originally found in https://github.com/astral-sh/ruff/pull/21685#issuecomment-3591738374. I bisected this to https://github.com/astral-sh/ruff/pull/20677.

---

_Label `bug` added by @ibraheemdev on 2025-12-01 23:44_

---

_Label `generics` added by @ibraheemdev on 2025-12-01 23:45_

---

_Assigned to @dcreager by @dcreager on 2025-12-01 23:47_

---

_Added to milestone `Beta` by @carljm on 2025-12-02 00:50_

---

_Closed by @dcreager on 2025-12-02 18:15_

---
