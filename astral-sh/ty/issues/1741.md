---
number: 1741
title: Emit diagnostics for unsupported comparisons involving non-fixed-length tuples
type: issue
state: closed
author: AlexWaygood
labels: []
assignees: []
created_at: 2025-12-03T12:20:21Z
updated_at: 2026-01-06T17:09:42Z
url: https://github.com/astral-sh/ty/issues/1741
synced_at: 2026-01-10T01:51:14Z
---

# Emit diagnostics for unsupported comparisons involving non-fixed-length tuples

---

_Issue opened by @AlexWaygood on 2025-12-03 12:20_

Pyright, mypy and pyrefly all emit diagnostics on all four comparisons here, but ty currently does not emit any diagnostics on this snippet:

```py
def f(
    a: tuple[int, ...],
    b: tuple[str, ...],
    c: tuple[str]
):
    a < b
    b < a
    a < c
    c < a
```

---

_Comment by @AlexWaygood on 2025-12-03 12:29_

we already emit diagnostics if both operands are _fixed_-length tuples, e.g.

```py
def f(
    x: tuple[int, int],
    y: tuple[str]
):
    x < y  # error: [unsupported-operator]
```

---

_Added to milestone `Stable` by @carljm on 2025-12-03 19:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-05 04:30_

---

_Closed by @charliermarsh on 2026-01-06 17:09_

---
