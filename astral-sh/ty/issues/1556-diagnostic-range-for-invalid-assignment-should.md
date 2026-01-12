```yaml
number: 1556
title: "Diagnostic range for `invalid-assignment` should only cover the right-hand side"
type: issue
state: closed
author: lucach
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-11-14T14:10:48Z
updated_at: 2025-11-18T15:19:01Z
url: https://github.com/astral-sh/ty/issues/1556
synced_at: 2026-01-12T15:54:25Z
```

# Diagnostic range for `invalid-assignment` should only cover the right-hand side

---

_@lucach_

### Summary


When an assignment has a type annotation, the blame for an invalid assignment should fall on the RHS expression, instead of the assignee. The current diagnostic message is already great ("Object of type `<rhs_type>` is not assignable to `<annotated_type>`") but its position is suboptimal.

Compare [ty on the Playground](https://play.ty.dev/38b330d6-ee5e-4049-9d1b-66ed966ffd97):

<img width="390" height="157" alt="Image" src="https://github.com/user-attachments/assets/2eb24657-80ed-427f-aa7b-94f426563520" />

with Pylance in VSCode:

<img width="390" height="157" alt="Image" src="https://github.com/user-attachments/assets/60247a0f-c064-4592-b984-435a6229009c" />

(The last statement is there to show that not all `invalid-assignment` diagnostics should automatically placed on the RHS. For those, the current behavior is already correct.)

### Version

5f501374c

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-14 14:15_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 14:17_

---

_Label `bug` added by @MichaReiser on 2025-11-14 14:17_

---

_Renamed from "Blame the RHS when an `invalid-assignment` is annotated" to "Diagnostic range for `invalid-assignment` should only cover the right-hand side" by @AlexWaygood on 2025-11-14 15:17_

---

_Assigned to @sharkdp by @sharkdp on 2025-11-15 19:49_

---

_Closed by @sharkdp on 2025-11-18 13:31_

---

_Comment by @sharkdp on 2025-11-18 14:01_

Thank you for reporting this! Fixed in https://github.com/astral-sh/ruff/pull/21476

---

_Comment by @lucach on 2025-11-18 15:19_

Thank _you_ for the excellent and rapid fix! 

---
