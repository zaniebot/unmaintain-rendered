```yaml
number: 16252
title: "[red-knot] Allow any `Ranged` argument for `report_lint` and `report_diagnostic`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/report-non-nodes
created_at: 2025-02-19T13:14:35Z
updated_at: 2025-02-19T13:34:58Z
url: https://github.com/astral-sh/ruff/pull/16252
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Allow any `Ranged` argument for `report_lint` and `report_diagnostic`

---

_@MichaReiser_

## Summary

Restricting `report_lint` and `report_diagnostic` to only take ast nodes as arguments is too restrictive. 
E.g., we may want to highlight a comparision like `10 not in b` but there's no single node that represents just the `not in ...` comparision. 


This PR relaxes the argument representing the range to accept any `Ranged` type.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-19 13:14_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-19 13:14_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-19 13:14_

---

_Label `red-knot` added by @MichaReiser on 2025-02-19 13:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4569 on 2025-02-19 13:18_

have these functions changed at all, or are they just moved to become methods in this PR?

---

_@AlexWaygood approved on 2025-02-19 13:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4569 on 2025-02-19 13:20_

I didn't change them (other than that I renamed them). I thought it's not worth another PR but I don't want this change to be part of my `__bool__` PR. But I should have called it out!

---

_@MichaReiser reviewed on 2025-02-19 13:20_

---

_@AlexWaygood reviewed on 2025-02-19 13:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4569 on 2025-02-19 13:22_

No worries! I'm fine with them being moved, just wanted to check that there wasn't any other change being made here that I needed to review 😁

---

_Merged by @MichaReiser on 2025-02-19 13:34_

---

_Closed by @MichaReiser on 2025-02-19 13:34_

---

_Branch deleted on 2025-02-19 13:34_

---
