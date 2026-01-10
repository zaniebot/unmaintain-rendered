---
number: 6059
title: "Formatter: Add parentheses around too long strings"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-25T09:57:10Z
updated_at: 2023-08-16T07:09:45Z
url: https://github.com/astral-sh/ruff/issues/6059
synced_at: 2026-01-10T01:22:45Z
---

# Formatter: Add parentheses around too long strings

---

_Issue opened by @konstin on 2023-07-25 09:57_

The formatter currently never adds parentheses around strings in the right hand side of an assignment. Black does add them if they help to match the line limit, otherwise it doesn't.

Input and identical ruff formatted output:

```python
DEFAULT_FILE_STORAGE_DEPRECATED_MSG = "The DEFAULT_FILE_STORAGE setting is deprecated. Use STORAGES instead."
DEFAULT_FILE_STORAGE_DEPRECATED_MSG = "The DEFAULT_FILE_STORAGE setting is deprecated. Use STORAGES instead. This makes it so long that parentheses don't help"
```

Black formatted output:

```python
DEFAULT_FILE_STORAGE_DEPRECATED_MSG = (
    "The DEFAULT_FILE_STORAGE setting is deprecated. Use STORAGES instead."
)
DEFAULT_FILE_STORAGE_DEPRECATED_MSG = "The DEFAULT_FILE_STORAGE setting is deprecated. Use STORAGES instead. This makes it so long that parentheses don't help"
```

---

_Label `formatter` added by @konstin on 2023-07-25 09:57_

---

_Referenced in [astral-sh/ruff#6069](../../astral-sh/ruff/issues/6069.md) on 2023-07-25 11:22_

---

_Referenced in [astral-sh/ruff#6203](../../astral-sh/ruff/issues/6203.md) on 2023-07-31 17:12_

---

_Referenced in [astral-sh/ruff#6271](../../astral-sh/ruff/issues/6271.md) on 2023-08-03 06:05_

---

_Comment by @MichaReiser on 2023-08-03 06:05_

Closing because it is a sub problem of https://github.com/astral-sh/ruff/issues/6271

---

_Closed by @MichaReiser on 2023-08-03 06:05_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:09_

---
