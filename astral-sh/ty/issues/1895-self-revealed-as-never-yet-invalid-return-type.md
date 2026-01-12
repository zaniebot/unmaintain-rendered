```yaml
number: 1895
title: "`Self` revealed as Never yet `invalid-return-type` error is reported"
type: issue
state: closed
author: stefanboca
labels:
  - control flow
  - type-inference
assignees: []
created_at: 2025-12-15T18:05:22Z
updated_at: 2025-12-16T09:34:32Z
url: https://github.com/astral-sh/ty/issues/1895
synced_at: 2026-01-12T15:54:26Z
```

# `Self` revealed as Never yet `invalid-return-type` error is reported

---

_@stefanboca_

### Summary

In this example, ty reveals a type of `Never` for `other` in `baz`, but still reports an `invalid-return-type` error.

```python
from typing import Self, final, reveal_type

@final
class Foo:
    def bar(self, other: Foo) -> bool:
        if isinstance(other, Foo): return True

    def baz(self, other: Self) -> bool:
        #                         ^^^^ Function can implicitly return `None`, which is not assignable to return type `bool` (invalid-return-type)
        if isinstance(other, Foo): return True
        reveal_type(other) # revealed: Never
```
`basedpyright` and`mypy`report no errors on this code snippet.

https://play.ty.dev/866a48cf-0624-49ba-b1cf-fb57a57cfd76

### Version

ty ruff/0.14.9+38 (4e1cf5747 2025-12-15)

---

_Comment by @carljm on 2025-12-16 01:24_

Ah, thanks for the report. What this indicates is that we are accurately recognizing the narrowing effect of the test `isinstance(other, Foo)` on the type of `other` (resulting in a type of `Never`), but we are not recognizing in type inference that the `isinstance` call will always return `True`. This should be easy to fix.

---

_Added to milestone `Stable` by @carljm on 2025-12-16 01:24_

---

_Label `control flow` added by @sharkdp on 2025-12-16 08:49_

---

_Label `type-inference` added by @sharkdp on 2025-12-16 08:49_

---

_Assigned to @sharkdp by @sharkdp on 2025-12-16 09:06_

---

_Closed by @sharkdp on 2025-12-16 09:34_

---
