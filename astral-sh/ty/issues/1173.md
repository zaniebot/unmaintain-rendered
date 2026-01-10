```yaml
number: 1173
title: Binding of Self within nested functions
type: issue
state: closed
author: Glyphack
labels:
  - bug
  - generics
assignees: []
created_at: 2025-09-11T20:42:07Z
updated_at: 2025-09-17T18:58:55Z
url: https://github.com/astral-sh/ty/issues/1173
synced_at: 2026-01-10T02:06:25Z
```

# Binding of Self within nested functions

---

_Issue opened by @Glyphack on 2025-09-11 20:42_

### Summary

In the following example when we annotate self with type var in f then f binds that type var and the `Self` in b is actually bound by `f`:

```py
from typing import Self

class C[T]():
    def f(self: Self):
        def b(x: Self):
            reveal_type(x)  # revealed: Self@f
```

If we annotate `self` with `C`:

```py
from typing import Self

class C():
    def f(self: "C"):
        def b(x: Self):
            reveal_type(x)  # revealed: Self@b
```

And the first example is correct based on [this todo comment](https://github.com/astral-sh/ruff/blob/59c8fda3f8f3bf2cc5c1ae34e7ca9dbea4d0278f/crates/ty_python_semantic/resources/mdtest/annotations/self.md#L33) in mdtests.s

### Version

_No response_

---

_Label `bug` added by @sharkdp on 2025-09-12 07:39_

---

_Label `generics` added by @sharkdp on 2025-09-12 07:39_

---

_Comment by @Glyphack on 2025-09-12 08:35_

I want to work on this issue.

---

_Assigned to @Glyphack by @sharkdp on 2025-09-12 08:37_

---

_Closed by @dcreager on 2025-09-17 18:58_

---
