```yaml
number: 742
title: "Support `NewType`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - typing semantics
assignees: []
created_at: 2025-07-01T14:01:32Z
updated_at: 2025-11-11T01:07:25Z
url: https://github.com/astral-sh/ty/issues/742
synced_at: 2026-01-10T02:06:24Z
```

# Support `NewType`

---

_Issue opened by @InSyncWithFoo on 2025-07-01 14:01_

`NewType` is an almost-zero-runtime-overhead way to create a new subtype:

```python
I64 = NewType('I64', int)
reveal_type(I64)    # Currently: NewType (correct, but lacks information)

takes_i64(42)       # error
takes_i64(I64(42))  # fine

def _(a: I64):
    reveal_type(a)  # I64
    takes_int(a)    # fine
```

One important thing to note is that `I64`'s type cannot be inferred as `type[I64]`, because at runtime it isn't:

```python
print(I64.__class__)  # 3.10 and later: NewType
                      # 3.9 and older: function

isinstance(42, I64)   # error
```

Specification: [Type aliases &sect; `NewType`](https://typing.python.org/en/latest/spec/aliases.html#newtype).

---

_Label `typing semantics` added by @AlexWaygood on 2025-07-01 14:03_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-07-24 21:24_

---

_Added to milestone `Beta` by @carljm on 2025-07-25 14:40_

---

_Unassigned @oconnor663 by @carljm on 2025-09-19 14:43_

---

_Assigned to @ibraheemdev by @carljm on 2025-09-19 15:00_

---

_Assigned to @oconnor663 by @MichaReiser on 2025-10-17 15:09_

---

_Unassigned @ibraheemdev by @MichaReiser on 2025-10-17 15:09_

---

_Closed by @oconnor663 on 2025-11-10 22:55_

---

_Comment by @oconnor663 on 2025-11-11 01:07_

All of the examples above are now working on `main`! With the exception of Python 3.9 support.

---
