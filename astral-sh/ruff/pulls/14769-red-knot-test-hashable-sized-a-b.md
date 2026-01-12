```yaml
number: 14769
title: "[red-knot] Test: Hashable/Sized => A/B"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/hashable-sized
created_at: 2024-12-04T13:41:03Z
updated_at: 2024-12-04T14:00:29Z
url: https://github.com/astral-sh/ruff/pull/14769
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Test: Hashable/Sized => A/B

---

_@sharkdp_

## Summary

Minor change that uses two plain classes `A` and `B` instead of `typing.Sized` and `typing.Hashable`.

The motivation is twofold: I remember that I was confused when I first saw this test. Was there anything specific to `Sized` and `Hashable` that was relevant here? (there is, these classes are not overlapping; and you can build a proper intersection from them; but that's true for almost all non-builtin classes).

I now ran into another problem while working on #14758: `Sized` and `Hashable` are protocols that we don't fully understand yet. This causing some trouble when trying to infer whether these are fully-static types or not.


---

_Label `red-knot` added by @sharkdp on 2024-12-04 13:41_

---

_Review requested from @carljm by @sharkdp on 2024-12-04 13:41_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-04 13:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-04 13:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:697 on 2024-12-04 13:42_

```suggestion
        // let's build 👷
```

---

_@AlexWaygood approved on 2024-12-04 13:44_

---

_Comment by @github-actions[bot] on 2024-12-04 13:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-12-04 14:00_

---

_Closed by @sharkdp on 2024-12-04 14:00_

---

_Branch deleted on 2024-12-04 14:00_

---
