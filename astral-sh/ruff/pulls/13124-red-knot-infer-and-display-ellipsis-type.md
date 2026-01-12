```yaml
number: 13124
title: "red-knot: infer and display ellipsis type"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-ellipsis-type
created_at: 2024-08-27T16:26:00Z
updated_at: 2025-05-07T15:24:00Z
url: https://github.com/astral-sh/ruff/pull/13124
synced_at: 2026-01-12T15:55:43Z
```

# red-knot: infer and display ellipsis type

---

_@chriskrycho_

## Summary

Just what it says on the tin: adds basic `EllipsisType` inference for any time `...` appears in the AST.

## Test Plan

Test that `x = ...` produces exactly what we would expect.


---

_Review requested from @carljm by @chriskrycho on 2024-08-27 16:26_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-27 16:26_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-27 16:26_

---

_Comment by @github-actions[bot] on 2024-08-27 16:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:305 on 2024-08-27 16:43_

I'm not sure this needs to be its own variant in the `Type` enum... it's conceptually just an instance of `types.EllipsisType`. `types.EllipsisType` was only exposed on Python 3.10+, but typeshed already provides a nice little compatibility workaround for us here: <https://github.com/python/typeshed/blob/f58dac1d62d33bb2255b762783d06463c40f5065/stdlib/builtins.pyi#L1849-L1865>

(`builtins.Ellipsis` is the same object as `...`)

```pycon
>>> ...
Ellipsis
>>> Ellipsis
Ellipsis
>>> Ellipsis is ...
True
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1340 on 2024-08-27 16:46_

I think I would probably just revert the changes you made to `types.rs` and `types/display.rs` and simply do this here:

```suggestion
        builtins_symbol_ty_by_name(self.db, "Ellipsis")
```

It doesn't seem to get us quite the right answer currently (the typeshed stubs there are using some features we don't support right now) so you may need to adjust the assertion in your test and add a TODO. But it'll set us up better in the long run.

---

_@AlexWaygood reviewed on 2024-08-27 16:46_

Hmm, not sure about this approach

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:305 on 2024-08-27 16:50_

One interesting way in which `...` _is_ special is that it's a singleton: it is the _only_ instance of `types.EllipsisType` that there is, or that there could be. But that can be thought of as just a sealed type that has exactly one member, and we've yet to implement a generalised way of handling sealed types. (We'll need them to tackle enums in Python!) See https://github.com/astral-sh/ruff/issues/12694.

---

_@AlexWaygood reviewed on 2024-08-27 16:50_

---

_@chriskrycho reviewed on 2024-08-27 19:27_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1340 on 2024-08-27 19:27_

That seems reasonable. The resulting display/result is `Unknown | Literal[EllipsisType]`. As I am leaving a TODO: what *should* it be?

---

_@AlexWaygood reviewed on 2024-08-27 19:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1340 on 2024-08-27 19:39_

It should just be `types.EllipsisType` (if on py310+) or `ellipsis` (if on Python <=3.9)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1342 on 2024-08-27 19:42_

I would eliminate this TODO. This TODO will be resolved by other added features; the code right here shouldn't need to change at all, which means we also don't need a reminder TODO comment here.

```suggestion
```

---

_@carljm approved on 2024-08-27 19:42_

---

_Merged by @AlexWaygood on 2024-08-27 19:52_

---

_Closed by @AlexWaygood on 2024-08-27 19:52_

---

_Branch deleted on 2024-08-27 19:54_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:44_

---
