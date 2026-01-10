```yaml
number: 200
title: "Add `implicit-reexport` rule for runtime files"
type: issue
state: open
author: dhruvmanila
labels:
  - imports
  - typing semantics
assignees: []
created_at: 2025-02-14T09:50:36Z
updated_at: 2025-11-18T16:10:21Z
url: https://github.com/astral-sh/ty/issues/200
synced_at: 2026-01-10T02:06:24Z
```

# Add `implicit-reexport` rule for runtime files

---

_Issue opened by @dhruvmanila on 2025-02-14 09:50_

### Description

Follow-up from https://github.com/astral-sh/ruff/issues/14099 which was closed by astral-sh/ruff#16073 which adds support for re-export conventions in stub files unconditionally.

We should add a new lint rule `implicit-reexport` which is:
* Disabled by default
* When enabled, performs the check for runtime files

For stub files, the implementation is such that it will ignore the symbol if it's not being re-exported which means that the symbol could be unbound as seen in [these tests](https://github.com/astral-sh/ruff/blob/60b3ef2c985dba405db58a72bc6f22ff64c249fa/crates/red_knot_python_semantic/resources/mdtest/import/conventions.md). But, for runtime files, we should be inferring the types correctly.

---

_Renamed from "[red-knot] Add `implicit-reexport` rule for runtime files" to "Add `implicit-reexport` rule for runtime files" by @MichaReiser on 2025-05-07 15:26_

---

_Label `imports` added by @AlexWaygood on 2025-05-11 07:50_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:50_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
