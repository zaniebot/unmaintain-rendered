```yaml
number: 1193
title: Duplicate diagnostic when there are multiple functions with same signatures
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-09-17T05:12:38Z
updated_at: 2025-11-14T08:54:12Z
url: https://github.com/astral-sh/ty/issues/1193
synced_at: 2026-01-12T15:54:24Z
```

# Duplicate diagnostic when there are multiple functions with same signatures

---

_@dhruvmanila_

### Summary

Consider the following source code:

```py
def f(x: int) -> None: ...

def _(x: str):
    f(x)

def f(x: int) -> None: ...

def f(x: int) -> None: ...
```

I see the following diagnostics for the `f(x)` call:

```
    Argument to function `f` is incorrect: Expected `int`, found `str` (invalid-argument-type) [Ln 4, Col 7]
    Argument to function `f` is incorrect: Expected `int`, found `str` (invalid-argument-type) [Ln 4, Col 7]
    Argument to function `f` is incorrect: Expected `int`, found `str` (invalid-argument-type) [Ln 4, Col 7]
```

Playground: https://play.ty.dev/8b8a0d58-1ba4-4bec-bb87-32179a6fa8e5

So, the number of diagnostics corresponds to the number of function definitions that exists which I think is because ty checks all function definitions. I think this is a bit confusing.

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-09-17 05:12_

---

_Label `diagnostics` added by @dhruvmanila on 2025-09-17 05:12_

---

_Comment by @carljm on 2025-09-17 16:15_

Yes, we consider the type of `f` within the nested scope to be the union of all reachable definitions in the outer scope, since it isn't feasible to do a full analysis of where in the outer control flow `_` could possibly be called. So I think this is really about more intelligent presentation of redundant-seeming errors in calls to union types. Though I'm not sure this specific example should motivate prioritization of those improvements; it seems like an unusual case.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:54_

---
