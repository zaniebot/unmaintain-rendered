```yaml
number: 15063
title: "[red-knot] Cleanup various `todo_type!()` messages"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-todo-messages
created_at: 2024-12-19T12:53:14Z
updated_at: 2024-12-19T13:05:11Z
url: https://github.com/astral-sh/ruff/pull/15063
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Cleanup various `todo_type!()` messages

---

_Pull request opened by @AlexWaygood on 2024-12-19 12:53_

## Summary

No functional change here -- this just improves our `todo_type!()` messages in several places. At some locations, this involves propagating an existing TODO message instead of overwriting it with a worse one (or overwriting it with no TODO message at all)

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-12-19 12:53_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-19 12:53_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-19 12:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-19 12:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:5034 on 2024-12-19 12:58_

Assuming I'm understanding the pattern correctly ;)

```suggestion
                todo_type!("`ReadOnly[]` type qualifier")
```

---

_@MichaReiser reviewed on 2024-12-19 12:58_

---

_@AlexWaygood reviewed on 2024-12-19 12:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5034 on 2024-12-19 12:58_

oops, thanks!!

---

_Merged by @AlexWaygood on 2024-12-19 13:03_

---

_Closed by @AlexWaygood on 2024-12-19 13:03_

---

_Branch deleted on 2024-12-19 13:03_

---

_Comment by @github-actions[bot] on 2024-12-19 13:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
