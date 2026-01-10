```yaml
number: 2141
title: assuming self-reference between function and return identifiers
type: issue
state: closed
author: diceroll123
labels: []
assignees: []
created_at: 2025-12-21T05:36:23Z
updated_at: 2025-12-21T08:53:17Z
url: https://github.com/astral-sh/ty/issues/2141
synced_at: 2026-01-10T01:56:41Z
```

# assuming self-reference between function and return identifiers

---

_Issue opened by @diceroll123 on 2025-12-21 05:36_

### Summary

I have two examples of this issue, not a new problem in ty, happened for as long as I've used it in the last few months.

```py
from dateutil import rrule  # type: ignore

class A:
    def rrule(cls) -> rrule.rrule:  # Function `rrule` has no attribute `rrule` (unresolved-attribute)
        ...
```

```py
from dateutil.rrule import rrule  # type: ignore

class A:
    def rrule(cls) -> rrule:  # Variable of type `def rrule(cls) -> Unknown` is not allowed in a type expression (invalid-type-form)
        ...
```

In playground: https://play.ty.dev/f820338c-0294-4a14-a04f-3a805b70e369

I can see how it could be considered intentional behavior, but I don't know if it is for ty, so I'm bringing it up. Thanks in advance!

### Version

0.0.5

---

_Comment by @MeGaGiGaGon on 2025-12-21 07:19_

I think this is the same as #580 (and I think there was another clearer issue but couldn't find it `:/`)

This happens because with the 3.14 possibly differed name semantics, the `rrule` annotation will always resolve to a class member first before the global import.

---

_Comment by @AlexWaygood on 2025-12-21 08:53_

Hey, thanks for the issue! Please see #1747

---

_Closed by @AlexWaygood on 2025-12-21 08:53_

---
