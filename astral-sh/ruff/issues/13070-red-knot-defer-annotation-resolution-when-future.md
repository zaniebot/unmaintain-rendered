```yaml
number: 13070
title: "[red-knot] defer annotation resolution when __future__.annotations is active"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2024-08-23T06:54:03Z
updated_at: 2024-09-19T17:09:23Z
url: https://github.com/astral-sh/ruff/issues/13070
synced_at: 2026-01-12T15:54:52Z
```

# [red-knot] defer annotation resolution when __future__.annotations is active

---

_@MichaReiser_

Red-knot panics on this:

```python
class IndexingMixin:
    def iloc(self) -> _iLocIndexer:
        return True


@doc(IndexingMixin.iloc)
class _iLocIndexer:
    pass
```

Changing the return type of `IndexingMixing.iloc` or changing the `_iLocIndexer` decorator to not reference `IndexingMixing.iloc` removes the panic

Source: https://github.com/pandas-dev/pandas/blob/0c24b20bd9cd743bd5f5afceb1a6fef54a2dbdc1/pandas/core/indexing.py#L4

---

_Label `bug` added by @MichaReiser on 2024-08-23 06:54_

---

_Label `red-knot` added by @MichaReiser on 2024-08-23 06:54_

---

_Renamed from "Red Knot panics when there's a cycle between a decorated class and a functions' return type" to "[red-knot] defer annotation resolution when __future__.annotations is active" by @carljm on 2024-09-03 21:32_

---

_Label `help wanted` added by @carljm on 2024-09-03 21:33_

---

_Comment by @carljm on 2024-09-03 21:33_

We have support now for deferring resolution of annotations, which is the first part of fixing this. The second part is to extend that support to any module where `from __future__ import annotations` is active; currently it only applies in a stub file.

---

_Closed by @MichaReiser on 2024-09-19 08:13_

---

_Comment by @carljm on 2024-09-19 16:48_

Confirmed that the above panic no longer occurs checking pandas. There is one other panic which is a shallow bug in with-item handling, I'll push a fix for it shortly.

---

_Comment by @carljm on 2024-09-19 16:56_

With this fix and https://github.com/astral-sh/ruff/pull/13409 we can check pandas with no panic.

---

_Comment by @charliermarsh on 2024-09-19 17:09_

Sweet

---
