```yaml
number: 19306
title: "BLE001: detect contextlib.suppress ?"
type: issue
state: open
author: spaceone
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-13T11:38:32Z
updated_at: 2025-07-14T15:49:00Z
url: https://github.com/astral-sh/ruff/issues/19306
synced_at: 2026-01-12T15:54:56Z
```

# BLE001: detect contextlib.suppress ?

---

_@spaceone_

```python
try:
    foo()
except BaseException:
    pass
```
is detected as `BLE001`, while the following isn't:

```python
with contextlib.suppress(BaseException):
    foo()
```

please consider if that would make sense to detect it.

---

_Label `rule` added by @MichaReiser on 2025-07-14 08:27_

---

_Label `needs-decision` added by @MichaReiser on 2025-07-14 08:27_

---

_Comment by @ntBre on 2025-07-14 15:49_

This came up a bit in https://github.com/astral-sh/ruff/pull/18213#pullrequestreview-2898656661 too.

---
