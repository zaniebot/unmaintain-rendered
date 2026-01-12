```yaml
number: 12782
title: "[red-knot] Move, rename and make public the `PyVersion` type"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: target-version
created_at: 2024-08-09T12:16:16Z
updated_at: 2024-08-09T15:49:18Z
url: https://github.com/astral-sh/ruff/pull/12782
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Move, rename and make public the `PyVersion` type

---

_@AlexWaygood_

## Summary

Working on https://github.com/astral-sh/ruff/pull/12759 has revealed that we need a general-purpose type that represents an arbitrary Python version (not necessarily one that we _support_), as well as a closed enumeration of supported Python versions. We actually already have a type like that -- but it's currently buried in `red_knot_python_semantic::module_resolver::typeshed::versions`. This PR publicly exposes that type, and moves both that type and the `TargetVersion` enum into a new `red_knot_python_semantic::python_version` submodule. This will allow the type to be used in https://github.com/astral-sh/ruff/pull/12759, and possibly elsewhere in the future as well.

## Test Plan

`cargo test -p red_knot_python_semantic`

---

_Label `red-knot` added by @AlexWaygood on 2024-08-09 12:16_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-09 12:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-09 12:16_

---

_Comment by @github-actions[bot] on 2024-08-09 12:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/python_version.rs`:5 on 2024-08-09 15:41_

Should this be `TargetVersion`?

```suggestion
/// TODO: unify with the `TargetVersion` enum in the linter/formatter crates?
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/python_version.rs`:57 on 2024-08-09 15:44_

We don't need to make this change in this PR but I think it's now a bit confusing when to use `TargetVersion` vs `PythonVersion`? Shouldn't all (or at least most) our code be open for new Python versions? Or to phrase it differently, what's the benefit of limiting `TargetVersion` to a fixed set? 

To me, the main reason why `TargetVersion` is an enum is just that it was easier to reference specific versions. But I think that can also be accomplished by `PythonVersion`, we can define constants for the versions that we care about (and we can implement `Default`). 



---

_@MichaReiser approved on 2024-08-09 15:44_

Merge conflicts ahead :laughing: 

---

_@AlexWaygood reviewed on 2024-08-09 15:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/python_version.rs`:5 on 2024-08-09 15:47_

The equivalent enum in the linter/formatter crates is called `PythonVersion`, e.g. https://github.com/astral-sh/ruff/blob/b595346213c5343c27fc8ff152f15b85c7b7d44d/crates/ruff_linter/src/settings/types.rs#L45, and that's what this comment is referring to

---

_@AlexWaygood reviewed on 2024-08-09 15:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/python_version.rs`:57 on 2024-08-09 15:48_

Hmm, maybe. Having it as an enum is also useful for command-line parsing. But you might be right that we could just use the `PythonVersion` type everywhere. Not sure; worth experimenting with as a followup, anyway!

---

_Renamed from "[red-knot] Move `red_knot_python_semantic::module_resolver::typeshed::versions::PyVersion`, and make it public" to "[red-knot] Move, rename and make public the `PyVersion` type" by @AlexWaygood on 2024-08-09 15:49_

---

_Merged by @AlexWaygood on 2024-08-09 15:49_

---

_Closed by @AlexWaygood on 2024-08-09 15:49_

---

_Branch deleted on 2024-08-09 15:49_

---
