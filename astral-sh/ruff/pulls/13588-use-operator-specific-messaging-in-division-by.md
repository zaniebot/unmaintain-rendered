```yaml
number: 13588
title: Use operator specific messaging in division by zero diagnostics
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/div-zero-msg
created_at: 2024-10-01T12:39:48Z
updated_at: 2024-10-01T13:58:40Z
url: https://github.com/astral-sh/ruff/pull/13588
synced_at: 2026-01-10T20:59:36Z
```

# Use operator specific messaging in division by zero diagnostics

---

_Pull request opened by @zanieb on 2024-10-01 12:39_

Requested at https://github.com/astral-sh/ruff/pull/13576#discussion_r1782530971

---

_Review requested from @carljm by @zanieb on 2024-10-01 12:39_

---

_Label `red-knot` added by @zanieb on 2024-10-01 12:39_

---

_Review requested from @MichaReiser by @zanieb on 2024-10-01 12:39_

---

_Review requested from @AlexWaygood by @zanieb on 2024-10-01 12:39_

---

_@zanieb reviewed on 2024-10-01 12:40_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:4227 on 2024-10-01 12:40_

This is the most consistent language I can find for this online.

---

_Comment by @github-actions[bot] on 2024-10-01 12:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4227 on 2024-10-01 12:56_

could possibly do something like this, but it's annoyingly verbose:

```suggestion
                "Using modulo with a zero divisor is undefined on an object of type 'Literal[3]'.",
```

---

_@AlexWaygood approved on 2024-10-01 12:56_

Thank you!!

---

_@zanieb reviewed on 2024-10-01 12:59_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:4227 on 2024-10-01 12:59_

Yeah that seems too verbose to me and differs quite a bit from the other messages.

---

_@AlexWaygood reviewed on 2024-10-01 13:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4227 on 2024-10-01 13:05_

I think what you have now is fine!

---

_Merged by @zanieb on 2024-10-01 13:58_

---

_Closed by @zanieb on 2024-10-01 13:58_

---

_Branch deleted on 2024-10-01 13:58_

---
