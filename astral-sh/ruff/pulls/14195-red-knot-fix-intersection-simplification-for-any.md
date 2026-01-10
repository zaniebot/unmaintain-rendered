```yaml
number: 14195
title: "[red-knot] Fix intersection simplification for `~Any`/`~Unknown`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-negative-any-simplification
created_at: 2024-11-08T08:12:02Z
updated_at: 2024-11-08T09:54:15Z
url: https://github.com/astral-sh/ruff/pull/14195
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Fix intersection simplification for `~Any`/`~Unknown`

---

_Pull request opened by @sharkdp on 2024-11-08 08:12_

## Summary

Another bug found using [property testing](https://github.com/astral-sh/ruff/pull/14178).

## Test Plan

New unit test

---

_Label `red-knot` added by @sharkdp on 2024-11-08 08:12_

---

_Review requested from @carljm by @sharkdp on 2024-11-08 08:12_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-08 08:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-08 08:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/builder.rs`:608 on 2024-11-08 08:12_

This would previously result in an unsimplified `Never & Any`.

---

_@sharkdp reviewed on 2024-11-08 08:12_

---

_@MichaReiser approved on 2024-11-08 08:16_

---

_Comment by @github-actions[bot] on 2024-11-08 08:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-11-08 09:54_

---

_Closed by @sharkdp on 2024-11-08 09:54_

---

_Branch deleted on 2024-11-08 09:54_

---
