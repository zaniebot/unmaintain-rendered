```yaml
number: 14232
title: "[red-knot] follow-ups to typevar types"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cleanups
created_at: 2024-11-09T19:06:19Z
updated_at: 2024-11-10T04:18:33Z
url: https://github.com/astral-sh/ruff/pull/14232
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] follow-ups to typevar types

---

_Pull request opened by @carljm on 2024-11-09 19:06_

A few follow-ups from post-land review of https://github.com/astral-sh/ruff/pull/14182.

The reason the special case for `NoDefaultType` in `Type::is_equivalent_to` wasn't needed to make the previous tests pass, is that `KnownClass::NoDefaultType.to_instance()` imports `typing_extensions._NoDefaultType` and then creates an instance type from it, rather than importing `typing_extensions.NoDefault` directly. Because `typing_extensions` does not import and re-export `typing._NoDefaultType` on versions where it exists (it only does this for `typing.NoDefault`), this means we only get `typing_extensions._NoDefaultType`, we never see `typing._NoDefaultType` at all.

This will require some further adjustments once we do understand `sys.version_info`, because then `typing_extensions._NoDefaultType` will be unbound on 3.13+ and we'll need an explicit fallback to import `typing._NoDefaultType` instead. (Or re-work this so that we import `NoDefault` directly -- that may be a reason to go back to using `KnownInstance` for this?) For now I just added a TODO for this, since we can't test any solutions here until we do have `sys.version_info` support.

The test I added here does fail without the special case in `is_equivalent_to`, because it directly imports the `NoDefault` singleton, so in the case where we import it from `typing_extensions`, we end up with an unsimplified union that displays as `NoDefault | NoDefault`.

---

_Label `red-knot` added by @carljm on 2024-11-09 19:06_

---

_Review requested from @MichaReiser by @carljm on 2024-11-09 19:06_

---

_Review requested from @AlexWaygood by @carljm on 2024-11-09 19:06_

---

_Review requested from @sharkdp by @carljm on 2024-11-09 19:06_

---

_Comment by @github-actions[bot] on 2024-11-09 19:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-11-10 01:13_

Thank you!!

---

_Merged by @carljm on 2024-11-10 04:18_

---

_Closed by @carljm on 2024-11-10 04:18_

---

_Branch deleted on 2024-11-10 04:18_

---
