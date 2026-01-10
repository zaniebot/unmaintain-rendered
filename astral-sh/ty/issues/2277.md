---
number: 2277
title: Emit diagnostic for any non-field (e.g. method or classmethod) defined on TypedDict
type: issue
state: closed
author: jack-mcivor
labels:
  - typeddict
assignees: []
created_at: 2025-12-30T15:11:54Z
updated_at: 2026-01-05T19:28:05Z
url: https://github.com/astral-sh/ty/issues/2277
synced_at: 2026-01-10T01:51:14Z
---

# Emit diagnostic for any non-field (e.g. method or classmethod) defined on TypedDict

---

_Issue opened by @jack-mcivor on 2025-12-30 15:11_

### Summary

Ty returns an unresolved-attribute error on TypedDict classmethods, although they appear to work fine at runtime. It's not really clear to me if they _should_ be allowed at run-time - [PEP-589](https://peps.python.org/pep-0589/) just says "Methods are not allowed". Classmethods like this are commonly used for alternative constructors (although it generally seems like a dataclass is a better option).

Perhaps ty should allow this? Or return a more explicit error?

Example:

```python
from __future__ import annotations
from typing import TypedDict

class D(TypedDict):
    a: int

    @classmethod
    def c(cls, a: int) -> D:
        return cls(a=a)

d = D.c(a=5)  # <- Class `D` has no attribute `c` (unresolved-attribute) [Ln 11, Col 5]
print(d.a)
```
returns
```sh
Class `D` has no attribute `c` (unresolved-attribute) [Ln 11, Col 5]
```

(link https://play.ty.dev/862cf628-892f-43bf-9263-66163f1e84d0)



### Version
ty 0.0.8

_No response_

---

_Comment by @jack-mcivor on 2025-12-30 15:14_

FWIW Mypy v1.19.1 throws two errors - one on the classmethod decorator & a similar attr-defined error on the `D.c(a=5)` site.
```
error: Invalid statement in TypedDict definition; expected "field_name: field_type"  [misc]
error: "type[D]" has no attribute "c"  [attr-defined]
```

---

_Assigned to @oconnor663 by @oconnor663 on 2025-12-30 17:19_

---

_Label `typeddict` added by @mtshiba on 2025-12-30 17:25_

---

_Comment by @AlexWaygood on 2025-12-30 18:01_

I don't think it should be a priority to provide support for `TypedDict` classmethods, since they are disallowed by the typing spec. But at the very least, we need to emit an explicit diagnostic saying that it's illegal, like mypy's `[misc]` diagnostic there. The current diagnostic is obviously pretty bad UX -- thanks for the report!

---

_Comment by @carljm on 2025-12-30 20:56_

Yeah, no other type checker allows these either; I don't think we should allow them unless the spec is amended to explicitly allow them.

---

_Renamed from "TypedDict with classmethod" to "Emit diagnostic for any non-field (e.g. method or classmethod) defined on TypedDict" by @carljm on 2025-12-30 20:58_

---

_Comment by @MeGaGiGaGon on 2025-12-30 21:00_

Just because I had some trouble tracking it down, for reference here's the current language of the spec:
https://typing.python.org/en/latest/spec/typeddict.html#class-based-syntax
> The body of the class definition defines the [items](https://typing.python.org/en/latest/spec/glossary.html#term-item) of the TypedDict type. It may also contain a docstring or `pass` statements (primarily to allow the creation of an empty TypedDict). No other statements are allowed, and type checkers should report an error if any are present. Type comments are not supported for creating TypedDict items.

---

_Closed by @oconnor663 on 2026-01-05 19:28_

---
