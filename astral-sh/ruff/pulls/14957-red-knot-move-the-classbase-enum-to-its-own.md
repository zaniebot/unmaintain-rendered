```yaml
number: 14957
title: "[red-knot] Move the `ClassBase` enum to its own submodule"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/move-class-base
created_at: 2024-12-13T12:49:09Z
updated_at: 2024-12-13T13:12:41Z
url: https://github.com/astral-sh/ruff/pull/14957
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Move the `ClassBase` enum to its own submodule

---

_@AlexWaygood_

## Summary

This enum is no longer an implementation detail of how we infer the MROs of classes, so it no longer feels appropriate for it to live in `mro.rs`. It's not very big right now, so we _could_ move it into `types.rs`. However, I think our general feeling is that `types.rs` and `infer.rs` are both already a little too big, and I also expect this enum to continue to grow as we add support for more special forms and generics. This PR therefore moves the enum to its own submodule, `class_base.rs`.

## Test Plan

All existing tests pass. No new ones are added; this is a pure refactor with no behavioural changes.


---

_Label `internal` added by @AlexWaygood on 2024-12-13 12:49_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-13 12:49_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-13 12:49_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-13 12:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-13 12:49_

---

_Comment by @github-actions[bot] on 2024-12-13 12:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-12-13 13:10_

---

_Merged by @AlexWaygood on 2024-12-13 13:12_

---

_Closed by @AlexWaygood on 2024-12-13 13:12_

---

_Branch deleted on 2024-12-13 13:12_

---
