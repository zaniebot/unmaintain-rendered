```yaml
number: 1129
title: confusing diagnostics for positional-only parameter supplied as keyword argument
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-09-04T17:00:08Z
updated_at: 2025-09-23T14:10:46Z
url: https://github.com/astral-sh/ty/issues/1129
synced_at: 2026-01-12T15:54:24Z
```

# confusing diagnostics for positional-only parameter supplied as keyword argument

---

_@carljm_

If you try to pass a positional-only parameter as a keyword argument:

```py
def f(x: int, /): ...

f(x=1)
```

We currently emit two different diagnostics:

```
    No argument provided for required parameter `x` of function `f` (missing-argument) [Ln 3, Col 1]
    Argument `x` does not match any known parameter of function `f` (unknown-argument) [Ln 3, Col 3]
```

This is potentially confusing -- it would be better if we emitted a clear diagnostic that the parameter is positional-only and can't be passed as a keyword argument.

---

_Label `diagnostics` added by @carljm on 2025-09-04 17:00_

---

_Label `help wanted` added by @MichaReiser on 2025-09-15 08:30_

---

_Closed by @AlexWaygood on 2025-09-23 14:10_

---
