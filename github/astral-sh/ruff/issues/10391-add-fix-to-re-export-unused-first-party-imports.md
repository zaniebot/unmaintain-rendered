---
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
synced_at: 2026-01-07T13:12:15-06:00
---

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

_Referenced in [astral-sh/ruff#10365](../../astral-sh/ruff/pulls/10365.md) on 2024-03-13 17:58_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-03-13 18:11_

---

_Label `help wanted` removed by @AlexWaygood on 2024-03-13 18:14_

---

_Referenced in [astral-sh/ruff#10390](../../astral-sh/ruff/issues/10390.md) on 2024-04-25 00:17_

---

_Referenced in [astral-sh/ruff#11168](../../astral-sh/ruff/pulls/11168.md) on 2024-04-26 22:45_

---

_Referenced in [astral-sh/ruff#11314](../../astral-sh/ruff/pulls/11314.md) on 2024-05-06 19:18_

---

_Closed by @plredmond on 2024-05-15 00:02_

---
