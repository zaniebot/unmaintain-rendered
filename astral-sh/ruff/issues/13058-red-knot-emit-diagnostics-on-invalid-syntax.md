```yaml
number: 13058
title: "[red-knot] emit diagnostics on invalid syntax"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-08-22T14:58:27Z
updated_at: 2024-08-23T06:22:13Z
url: https://github.com/astral-sh/ruff/issues/13058
synced_at: 2026-01-12T15:54:52Z
```

# [red-knot] emit diagnostics on invalid syntax

---

_@carljm_

We regularly hit confusion because of the error-recovering parser letting syntax errors pass silently, and then red-knot behaving in unexpected ways because we didn't realize there was a syntax error in the code. We should emit diagnostics on syntax errors to avoid this.

Related: #12923


---

_Label `red-knot` added by @carljm on 2024-08-22 14:58_

---

_Comment by @MichaReiser on 2024-08-22 15:21_

We already do this but in `lint_syntax`.

https://github.com/astral-sh/ruff/blob/dce87c21fdf73a58f3821cae5e71b9da234e29ce/crates/red_knot_workspace/src/lint.rs#L40-L64

We should move this into `check_file`. 

---

_Closed by @MichaReiser on 2024-08-23 06:22_

---
