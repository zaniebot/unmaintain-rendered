```yaml
number: 1324
title: Type of inherited attribute is lost when using dynamically constructed base class
type: issue
state: closed
author: vlashada
labels:
  - question
assignees: []
created_at: 2025-10-08T09:54:11Z
updated_at: 2025-11-24T04:52:44Z
url: https://github.com/astral-sh/ty/issues/1324
synced_at: 2026-01-10T01:58:59Z
```

# Type of inherited attribute is lost when using dynamically constructed base class

---

_Issue opened by @vlashada on 2025-10-08 09:54_

### Summary

When returning a class type (type[A]) from a function and using it as a dynamic base class, ty fails to preserve attribute type information in the subclass. The inherited attribute is recognized as unknown instead of the expected static type.

```python
from typing import reveal_type

class A:
    name: str

def make_a() -> type[A]:
    return A

class B(make_a()): # Unsupported class base with type `type[A]` [unsupported-base]
    pass

a = A()
reveal_type(a.name) # Revealed type: `str`
b = B()
reveal_type(b.name) # Revealed type: `unknown`
```

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `question` added by @sharkdp on 2025-10-08 10:42_

---

_Comment by @sharkdp on 2025-10-08 10:46_

Thank you for reporting this. Using `type[A]` as a type for a base class seems wrong to me? `type[A]` is a type that includes the class `A` itself, but also all subclasses of `A`. So we can't really know what a base class of type `type[A]` means. Or is this specified somewhere to be a supported typing pattern?

What you would really want to have is to return a type that *only* includes the class `A` itself. This can't be spelled out using normal Python syntax/types, though. In `ty` in particular, you could write it as `TypeOf[A]` (see [this playground](https://play.ty.dev/8ee5ef97-729e-4db2-8392-02c257afc1db)), but you probably don't want to rely on `ty_extensions` in a real code base.

---

_Comment by @sharkdp on 2025-10-08 10:56_

For reference, both mypy and pyrefly refuse your code as well ("Unsupported dynamic base class", "Invalid expression form for base class: `make_a()`"). pyright seems to accept this code and treat it as if `A` would be the base class. However, if I recall correctly, pyright does treat `type[A]` as being synonymous with "the type that only includes the class `A` itself" in other cases as well.

---

_Comment by @vlashada on 2025-10-08 16:44_

The type of `b.name` is still unknown when using `ty_extensions.TypeOf` as in your playground example?

---

_Comment by @carljm on 2025-10-08 16:52_

> The type of `b.name` is still unknown when using `ty_extensions.TypeOf` as in your playground example?

No, the type of `b.name` is revealed as `str` in the playground example. Are you maybe looking at the comment in the code sample (which is out of date) instead of the actual diagnostic emitted by ty (in the lower pane)?

---

_Closed by @vlashada on 2025-11-24 04:52_

---
