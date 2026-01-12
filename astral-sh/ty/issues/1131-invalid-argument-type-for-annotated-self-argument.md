```yaml
number: 1131
title: "invalid-argument-type for annotated `self` argument when a generic context exists"
type: issue
state: closed
author: Glyphack
labels:
  - bug
  - generics
assignees: []
created_at: 2025-09-05T11:27:09Z
updated_at: 2025-09-08T14:57:10Z
url: https://github.com/astral-sh/ty/issues/1131
synced_at: 2026-01-12T15:54:24Z
```

# invalid-argument-type for annotated `self` argument when a generic context exists

---

_@Glyphack_

This came up as part of https://github.com/astral-sh/ruff/pull/18007. Since it can be reproduced on main I created a separate issue & PR to check ecosystem changes.

Normally annotated self accepts instance of that class a valid argument:

```py
from typing import Self

class C:
    def m(self: Self, x: int) -> None: ...

c: C = C()
c.m(1)
```

But if the method has generics then this would not work:

```py
from typing import Self
class C:
    def m[S](self: Self, x: S) -> None: ...

c: C[int] = C()
# error: [invalid-argument-type] "Argument to bound method `m` is incorrect: Expected `Self@m`, found `C`"
c.m(1)
```

---

_Comment by @sharkdp on 2025-09-05 11:52_

> c: C[int] = C()

I know it's not the key issue here, but it also looks like a bug that you're allowed to [declare this as `C[int]` when `C` itself is not a generic class](https://play.ty.dev/98ea4515-8f3b-4223-984f-254b089f0e6b). You can probably remove that annotation from your example.

---

_Label `generics` added by @sharkdp on 2025-09-05 11:52_

---

_Label `bug` added by @sharkdp on 2025-09-08 11:31_

---

_Comment by @sharkdp on 2025-09-08 11:46_

In the following example, we can see how `Self` vanishes from the generic context when using a PEP 695 typevar:

https://play.ty.dev/7208eaba-cf11-4352-9c9c-46f6968f3aa3
```py
from typing import Self, TypeVar, reveal_type
from ty_extensions import generic_context

_T = TypeVar("_T")


class C:
    def method_with_non_generic_param(self: Self, x: int) -> None: ...
    def method_with_legacy_typevar(self: Self, x: _T) -> None: ...
    def method_with_pep695_typevar[T](self: Self, x: T) -> None: ...


reveal_type(generic_context(C.method_with_non_generic_param))  # tuple[Self@method_with_non_generic_param]
reveal_type(generic_context(C.method_with_legacy_typevar))  # tuple[Self@method_with_legacy_typevar, _T@method_with_legacy_typevar]
reveal_type(generic_context(C.method_with_pep695_typevar))  # tuple[T@method_with_pep695_typevar]
```

---

_Assigned to @sharkdp by @sharkdp on 2025-09-08 11:54_

---

_Closed by @sharkdp on 2025-09-08 14:57_

---
