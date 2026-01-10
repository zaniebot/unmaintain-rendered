```yaml
number: 443
title: division-by-zero error raised when conditional statements check for division by 0
type: issue
state: closed
author: DirkNZ
labels:
  - bug
  - control flow
  - unreachable-code
assignees: []
created_at: 2025-05-18T23:10:04Z
updated_at: 2025-05-26T12:27:21Z
url: https://github.com/astral-sh/ty/issues/443
synced_at: 2026-01-10T02:34:10Z
```

# division-by-zero error raised when conditional statements check for division by 0

---

_Issue opened by @DirkNZ on 2025-05-18 23:10_

### Summary

When using a conditional check to cover the case where you might divide by zero, ty raises a division-by-zero error. MyPy will pass.

Example: [Playground](https://play.ty.dev/8f7eb7d5-acb5-41fe-95c8-08f725dfd812)

```py
a: int = 1
b: int = 0

c = a / b if b > 0 else 0

if b > 0:
    c = a / b
else:
    c = 0

# mypy passes in both
# ty fails in both:
# error[division-by-zero]: Cannot divide object of type `Literal[1]` by zero
#  --> type_example.py:4:5
#   |
# 2 | b: int = 0
# 3 |
# 4 | c = a / b if b > 0 else 0
#   |     ^^^^^
#   |
# info: rule `division-by-zero` is enabled by default`
```

### Version

ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)

---

_Label `bug` added by @sharkdp on 2025-05-19 07:44_

---

_Label `control flow` added by @sharkdp on 2025-05-19 07:44_

---

_Comment by @sharkdp on 2025-05-19 07:45_

Thank you for reporting this.

Internal note: we detect those expressions/statements as unreachable, but we do not silence the division-by-zero diagnostic.

---

_Comment by @tonka3000 on 2025-05-19 20:16_

Not 100% sure but this could be another example

```py
from enum import IntEnum


class DesignState(IntEnum):
    SUCCESS = 0
    ERROR = 1


class Design:
    def __init__(self, state: DesignState):
        self.state: DesignState = state


def stats(designs: list[Design]):
    feasible = 0
    error = 0
    total = 0
    for d in designs:
        total += 1
        if d.state == DesignState.SUCCESS:
            feasible += 1
        elif d.state == DesignState.ERROR:
            error += 1

    feasible_factor = feasible / total if total > 0 else 0.0 # Cannot divide object of type `Literal[0]` by zeroty(division-by-zero) , Cannot divide object of type `Literal[1]` by zeroty(division-by-zero)
```

false positive message are:

- Cannot divide object of type `Literal[0]` by zeroty(division-by-zero)
- Cannot divide object of type `Literal[1]` by zeroty(division-by-zero)

This should not happend here because `total > 0` check is there.

---

_Comment by @carljm on 2025-05-19 22:13_

Yes, the latter case is because we don't yet implement narrowing for `>` or `<` tests; we should be able to eliminate some `Literal` types with such narrowing. Though tbh my feeling is we should probably disable `division-by-zero` by default; it is prone to the same sort of "if our control flow and narrowing is not perfect, it gives false positives" problems as `possibly-unresolved-reference` is.

---

_Comment by @MichaReiser on 2025-05-20 05:17_

> it is prone to the same sort of "if our control flow and narrowing is not perfect, it gives false positives" problems as possibly-unresolved-reference is.

Isn't this true for many diagnostics. Even invalid call diagnostics are possible because of incorrect narrowing (if the signature is strict enough)

---

_Comment by @carljm on 2025-05-20 14:41_

> Isn't this true for many diagnostics.

In general, yes. In practice, there are significant qualitative differences. Diagnostics (such as divide-by-zero) that rely for their correctness on distinguishing very particular literal values from each other require more and different kinds of narrowing to be supported. For example, neither mypy nor pyright support narrowing a union of literal integers based on an inequality test; this narrowing just rarely turns out to be useful, except for divide-by-zero. Not coincidentally, neither one supports a divide-by-zero diagnostic, either.

I'm not in principle opposed to implementing support for inequality narrowing and unreachable-code silencing and seeing if that gets us close enough for divide-by-zero to work well. But I don't think either inequality-narrowing nor divide-by-zero should be a priority at this point, and I do think we should disable divide-by-zero until we are ready to prioritize reducing its false positives.

---

_Label `unreachable-code` added by @sharkdp on 2025-05-26 11:43_

---

_Comment by @sharkdp on 2025-05-26 12:27_

I think this can be closed, now that we don't emit this diagnostic by default: https://play.ty.dev/d4f6b510-383d-47ef-8edc-f9e232245410

We will further track the core issue in #443. I added a link to this ticket as an example.

---

_Closed by @sharkdp on 2025-05-26 12:27_

---
