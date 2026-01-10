---
number: 10509
title: "[v0.3.3 regression]: F811 incorrectly emitted if a symbol in a stub is re-exported and a symbol by the same name is defined in a different scope"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - linter
assignees: []
created_at: 2024-03-21T12:00:04Z
updated_at: 2024-03-21T16:22:51Z
url: https://github.com/astral-sh/ruff/issues/10509
synced_at: 2026-01-10T01:22:50Z
---

# [v0.3.3 regression]: F811 incorrectly emitted if a symbol in a stub is re-exported and a symbol by the same name is defined in a different scope

---

_Issue opened by @AlexWaygood on 2024-03-21 12:00_

Starting in v0.3.3, Ruff incorrectly emits F811 on this snippet if it's a `.pyi` file:

```py
from foo import Bar as Bar

class Eggs:
    Bar: int  # F811 Redefinition of unused `Bar` from line 1
```

This shouldn't be flagged as a redefinition, as the `Eggs.Bar` variable is being defined in a different scope to the global `Bar` variable, which is being re-exported from the module. The regression bisects to 4b0666919 (by me).

---

_Label `bug` added by @AlexWaygood on 2024-03-21 12:00_

---

_Label `linter` added by @AlexWaygood on 2024-03-21 12:00_

---

_Assigned to @charliermarsh by @AlexWaygood on 2024-03-21 15:18_

---

_Referenced in [astral-sh/ruff#10512](../../astral-sh/ruff/pulls/10512.md) on 2024-03-21 16:07_

---

_Closed by @charliermarsh on 2024-03-21 16:22_

---

_Referenced in [astral-sh/ruff#10564](../../astral-sh/ruff/pulls/10564.md) on 2024-03-25 10:18_

---
