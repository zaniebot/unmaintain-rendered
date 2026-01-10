```yaml
number: 10391
title: "Add fix to re-export unused first-party imports in `__init__.py` files"
type: issue
state: closed
author: zanieb
labels:
  - fixes
assignees: []
created_at: 2024-03-13T17:56:53Z
updated_at: 2024-05-15T00:02:35Z
url: https://github.com/astral-sh/ruff/issues/10391
synced_at: 2026-01-10T11:09:52Z
```

# Add fix to re-export unused first-party imports in `__init__.py` files

---

_Issue opened by @zanieb on 2024-03-13 17:56_

See https://github.com/astral-sh/ruff/issues/10365#issuecomment-1995132836 and #10390 

We currently do not offer any fixes for unused imports in `__init__.py` files.

This issue is to fix first-party imports by 

1. Adding them to `__all__` if `__all__` is present
2. Otherwise, converting them to a redundant alias e.g. `import foo as foo`.

We can probably label this fix as safe.
            

---

_Label `fixes` added by @zanieb on 2024-03-13 17:57_

---

_Label `help wanted` added by @zanieb on 2024-03-13 17:57_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-03-13 18:11_

---

_Label `help wanted` removed by @AlexWaygood on 2024-03-13 18:14_

---

_Closed by @plredmond on 2024-05-15 00:02_

---
