```yaml
number: 17488
title: "[red-knot] Detect (some) invalid protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/invalid-protocols
created_at: 2025-04-19T22:08:33Z
updated_at: 2025-04-22T11:56:07Z
url: https://github.com/astral-sh/ruff/pull/17488
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Detect (some) invalid protocols

---

_@AlexWaygood_

## Summary

Stacked on top of #17487.

If a class includes `Protocol` or `Protocol[]` in its bases, it cannot also include any non-protocol classes in its bases, or `TypeError` is raised at runtime. This PR adds the logic to detect this error by adding a new lint, `invalid-protocol`.

## Test Plan

existing mdtests updated


---

_Label `red-knot` added by @AlexWaygood on 2025-04-19 22:08_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-19 22:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-19 22:08_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-19 22:08_

---

_Comment by @github-actions[bot] on 2025-04-19 22:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:793 on 2025-04-21 14:55_

Not sure if it makes sense to add new uses of `report_lint_old`?

But I guess there is some argument for keeping consistency with nearby code, and changing them all together?

cc @BurntSushi 

---

_@carljm approved on 2025-04-21 14:55_

Looks good!

---

_@AlexWaygood reviewed on 2025-04-21 15:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:793 on 2025-04-21 15:23_

Heh, yeah, I also wondered about this... I think I'll keep it as-is for now, purely for local consistency if nothing else 😄 apologies to @BurntSushi if this causes annoyance for him!

---

_Merged by @AlexWaygood on 2025-04-21 15:24_

---

_Closed by @AlexWaygood on 2025-04-21 15:24_

---

_Branch deleted on 2025-04-21 15:24_

---

_@BurntSushi reviewed on 2025-04-22 11:56_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/infer.rs`:793 on 2025-04-22 11:56_

Yeah it's okay, I'll fix it up. Thanks for highlighting this!

---
