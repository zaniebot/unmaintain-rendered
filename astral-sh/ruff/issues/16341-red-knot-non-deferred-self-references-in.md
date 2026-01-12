```yaml
number: 16341
title: "[red-knot] Non-deferred self-references in annotations"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2025-02-24T08:37:18Z
updated_at: 2025-03-28T15:11:58Z
url: https://github.com/astral-sh/ruff/issues/16341
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Non-deferred self-references in annotations

---

_@sharkdp_

### Description

We currently do not emit a diagnostic for something like the following, where a class refers to itself in a type annotation (without `from __future__ import annotations`, not in a stub file, and without stringifying it as `"C"`):
```py
class C:
    def f(self) -> C:
        ...
```
This leads to a `NameError` at runtime, so we should ideally detect it.

---

_Label `red-knot` added by @sharkdp on 2025-02-24 08:37_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-02-24 17:30_

---

_Comment by @carljm on 2025-03-20 22:49_

It looks like there are two separate bugs involved here; see https://github.com/astral-sh/ruff/pull/16861#discussion_r2006569021

---

_Removed from milestone `Red Knot Q1 2025` by @carljm on 2025-03-27 18:35_

---

_Added to milestone `Red Knot Beta` by @carljm on 2025-03-27 18:35_

---

_Removed from milestone `Red Knot Beta` by @carljm on 2025-03-27 18:36_

---

_Added to milestone `Red Knot Public Release` by @carljm on 2025-03-27 18:36_

---

_Closed by @carljm on 2025-03-28 15:11_

---
