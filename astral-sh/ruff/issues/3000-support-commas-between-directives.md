```yaml
number: 3000
title: Support commas between directives
type: issue
state: closed
author: NeilGirdhar
labels: []
assignees: []
created_at: 2023-02-17T23:23:38Z
updated_at: 2023-02-17T23:24:17Z
url: https://github.com/astral-sh/ruff/issues/3000
synced_at: 2026-01-12T15:54:43Z
```

# Support commas between directives

---

_@NeilGirdhar_

It would a bit nicer to support semicolons between directives:
```python
value_state = value._state  # pylint: disable=protected-access; noqa: SLF001
```
in addition to:
```python
value_state = value._state  # pylint: disable=protected-access # noqa: SLF001
```

---

_Closed by @NeilGirdhar on 2023-02-17 23:23_

---

_Comment by @NeilGirdhar on 2023-02-17 23:24_

Come to think of it, if we had https://github.com/charliermarsh/ruff/issues/2999, we don't need this.

---
