```yaml
number: 197
title: "how should `assert_type` handle the float/complex special case?"
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-02-14T22:18:50Z
updated_at: 2025-05-23T09:48:32Z
url: https://github.com/astral-sh/ty/issues/197
synced_at: 2026-01-10T02:34:09Z
```

# how should `assert_type` handle the float/complex special case?

---

_Issue opened by @carljm on 2025-02-14 22:18_

### Description

We follow the typing spec and other type checkers in implementing the special case that a `float` annotation means `int | float`, and a `complex` annotation means `int | float | complex`.

We don't currently follow other type checkers in trying to "hide" this special case by revealing just `float` or `complex` when the actual type we have is `int | float` or `int | float | complex`. Doing this leads to very strange-looking behaviors, like this from pyright:

```py
def _(x: float):
    reveal_type(x)  # float
    if isinstance(x, float):
        reveal_type(x)  # float
    reveal_type(x)  # float | int ??
```

But this leads us to fail this type assertion:

```py
assert_type(3.14, float)
```

Because we interpret `float` there as a type expression, meaning we take it to be `int | float`, but we actually infer just `float` for the literal.

---

_Comment by @InSyncWithFoo on 2025-02-15 00:33_

I think `float` should be interpreted as `int | float` no matter the context. That helps with consistency.

Instead, `knot_extensions` should export `BareFloat` and `BareComplex` as public APIs for such use cases (credits go to @sharkdp):

```python
type BareFloat = TypeOf[1.0]
type BareComplex = TypeOf[1j]
```

---

_Renamed from "[red-knot] how should `assert_type` handle the float/complex special case?" to "how should `assert_type` handle the float/complex special case?" by @MichaReiser on 2025-05-07 15:26_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:51_

---

_Comment by @sharkdp on 2025-05-23 09:48_

I think we still haven't decided if `ty_extensions` should be user-facing, but for now, `ty_extensions.JustFloat` and `ty_extensions.JustComplex` are available:

```py
from ty_extensions import JustFloat

def only_float_allowed(f: JustFloat): ...

only_float_allowed(1.0)
only_float_allowed(1)  # invalid_argument-type
```
https://play.ty.dev/e83982c2-6ab6-409a-8608-97f7f0baa14a

---

_Closed by @sharkdp on 2025-05-23 09:48_

---
