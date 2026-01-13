```yaml
number: 2482
title: "Improve diagnostics for class definitions that don't provide required arguments to superclass `__init_subclass__` methods"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2026-01-13T16:15:43Z
updated_at: 2026-01-13T16:16:03Z
url: https://github.com/astral-sh/ty/issues/2482
synced_at: 2026-01-13T16:27:19Z
```

# Improve diagnostics for class definitions that don't provide required arguments to superclass `__init_subclass__` methods

---

_@AlexWaygood_

I wish we could make the diagnostics here a bit friendlier. Some users may not be aware of how the semantics of `__init_subclass__` work, and the diagnostic doesn't try to explain it much at the moment:

---

<img width="1964" height="736" alt="image" src="https://github.com/user-attachments/assets/1508dbed-3f67-4449-bc00-71423051412e" />

---

But propagating context into (or out of) `binding.report_diagnostics()` in order to add subdiagnostics looks really complicated ☹️. So that's probably better done as a followup

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/22185#pullrequestreview-3656633344_
            

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-13 16:16_

---
