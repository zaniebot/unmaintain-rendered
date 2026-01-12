```yaml
number: 15377
title: "[red-knot] refactor Outcome enums"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-09T17:48:28Z
updated_at: 2025-02-19T20:35:24Z
url: https://github.com/astral-sh/ruff/issues/15377
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] refactor Outcome enums

---

_@carljm_

This in particular needs to include simplifying the `CallOutcome` API and making it more consistent in terms of which methods emit diagnostics and which just return a result for the caller to decide.

---

_Label `red-knot` added by @carljm on 2025-01-09 17:48_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:48_

---

_Assigned to @AlexWaygood by @carljm on 2025-01-09 17:48_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-02-19 20:34_

---

_Unassigned @AlexWaygood by @MichaReiser on 2025-02-19 20:34_

---

_Comment by @MichaReiser on 2025-02-19 20:35_

This is done. There are probably more `Type::` methods that require the same treatment but `call` is done.

---

_Closed by @MichaReiser on 2025-02-19 20:35_

---
