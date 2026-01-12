```yaml
number: 2140
title: "assume in narrowing that tuple subclasses don't override `__eq__`"
type: issue
state: closed
author: dsfaccini
labels:
  - narrowing
assignees: []
created_at: 2025-12-21T04:34:55Z
updated_at: 2025-12-22T22:07:58Z
url: https://github.com/astral-sh/ty/issues/2140
synced_at: 2026-01-12T15:54:26Z
```

# assume in narrowing that tuple subclasses don't override `__eq__`

---

_@dsfaccini_

### Summary
Ty does not narrow a `Literal`/tuple union when assigning to a wider `Literal` union.

Playground link: https://play.ty.dev/f8ecb06d-124e-4921-a082-9038c1302ed2

### Repro (self-contained)
`ty_repro_literal.py`:
```python
from typing import Literal

def demo(x: Literal['none', 'auto', 'required'] | tuple[list[str], Literal['auto', 'required']]):
    y: Literal['none', 'required', 'auto'] | int | None
    if x in ('auto', 'required'):
        y = x
    elif x == 'none':
        y = 'none'
    elif isinstance(x, tuple):
        y = 'auto'
    else:
        y = None
    return y
```

Run:
```bash
uvx ty check ty_repro_literal.py
```

### Actual result
```
error[invalid-assignment]: Object of type `Literal["auto", "required"] | tuple[list[str], Literal["auto", "required"]]` is not assignable to `Literal["none", "required", "auto"] | int | None`
 --> ty_repro_literal.py:6:9
```

### Expected result
No diagnostic: in the `x in ('auto', 'required')` branch, `x` is narrowed to `Literal['auto', 'required']`, which is assignable to `Literal['none', 'required', 'auto'] | int | None`.

### Notes
Occurs on `ty 0.0.4` via `uvx ty check`.


---

_Label `narrowing` added by @mtshiba on 2025-12-21 05:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-21 13:42_

---

_Comment by @carljm on 2025-12-22 18:05_

Thanks for the report!

ty is working as designed in this case, and is in fact highlighting a hole in the logic of your code.

The type `tuple` includes subclasses of the built-in tuple, and such subclasses are free to arbitrarily override `__eq__` in such a way that they would compare equal to the string "auto" or the string "required", thus passing the `if x in ('auto', 'required')` check. Thus if we add a `reveal_type(x)` inside the first `if` body, we can see that ty narrows away the possibility of `Literal['none']`, but it does not narrow away the possibility of the tuple type, and this leads to the error on assigning to `y`.

Adding an `isinstance(x, str)` condition to that first `if` test closes this hole, and the error goes away.

---

_Closed by @carljm on 2025-12-22 18:05_

---

_Comment by @AlexWaygood on 2025-12-22 18:09_

> The type `tuple` includes subclasses of the built-in tuple, and such subclasses are free to arbitrarily override `__eq__`

Though I think we're planning to ban `__eq__` overrides (and overrides of other comparison methods) on tuple subclasses, or many of the `Literal` types we infer for tuple comparisons aren't sound?

---

_Comment by @AlexWaygood on 2025-12-22 19:10_

I think that ban would mean that the requested narrowing here would in fact be sound. Even though `tuple[list[str], Literal["auto", "required"]]` is not a single-valued type (due to the `list[str]` element), if we "know" that `tuple.__eq__` was not overridden on any subclass of `tuple` then we can be confident that an object with that type will never compare equal to an object of type `Literal["auto"]` or an object of type `Literal["required"]`.

---

_Comment by @carljm on 2025-12-22 19:51_

Yep, agreed. I remembered discussion of banning `__bool__` and `__len__` on tuple subclasses (in #215 and #560); I'm not finding explicit discussion of banning `__eq__` also, but I think you're right that we do other narrowing that's only valid if we do.

So I'll reopen this (and review Charlie's PR!). Also created #2171 to explicitly track our intention re tuple subclasses. I thought maybe we had an issue for this already, but I couldn't find one.

---

_Reopened by @carljm on 2025-12-22 19:51_

---

_Renamed from "ty literal narrowing: invalid-assignment after Literal branch test" to "assume in narrowing that tuple subclasses don't override `__eq__`" by @carljm on 2025-12-22 19:51_

---

_Closed by @charliermarsh on 2025-12-22 20:44_

---

_Comment by @dsfaccini on 2025-12-22 22:07_

thank you all for checking this out

---
