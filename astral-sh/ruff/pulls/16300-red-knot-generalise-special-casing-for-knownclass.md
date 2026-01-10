```yaml
number: 16300
title: "[red-knot] Generalise special-casing for `KnownClass`es in `Type::bool`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-bool-generalise
created_at: 2025-02-21T11:48:53Z
updated_at: 2025-02-21T20:46:39Z
url: https://github.com/astral-sh/ruff/pull/16300
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Generalise special-casing for `KnownClass`es in `Type::bool`

---

_Pull request opened by @AlexWaygood on 2025-02-21 11:48_

## Summary

This generalises the special casing added in https://github.com/astral-sh/ruff/pull/16292 to a `match` statement on all `KnownClass` variants. Even if this doesn't actually result in a noticeable speedup, I kind-of prefer it over the current version on `main` because:
- It means we have more precise type inference: we now understand that `sys.version_info` and `builtins.Ellipsis` are always truthy
- It feels more extensible, and more maintainable (we won't forget to add special-casing for new `KnownClass` variants if we add any in the future)
- By doing one `class.known(db)` call in `Type::bool()` and then matching on the result (rather than doing two `class.is_known(db, KnownClass::Whatever)` calls), we now only do one Salsa db lookup instead of two

In addition to the change to `Type::bool()`, I also moved the `match` statement over `KnownClass` variants in `Type::is_single_valued()` to a new method on `KnownClass`.

## Test Plan

Added some new mdtests asserting that instance types for these known classes are understood to be subtypes of `AlwaysTruthy`.

---

_Label `red-knot` added by @AlexWaygood on 2025-02-21 11:48_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-21 11:48_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-21 11:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-21 11:48_

---

_@AlexWaygood reviewed on 2025-02-21 11:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3208 on 2025-02-21 11:50_

An unrelated bug that was exposed by the tests I added in this PR... turns out, we were never inferring `KnownClass::TypeVar` for any classes before now 😶

---

_Renamed from "[red-knot] Generalise special-casing for KnownClasses in `Type::boo…" to "[red-knot] Generalise special-casing for `KnownClass`es in `Type::bool`" by @AlexWaygood on 2025-02-21 11:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1663 on 2025-02-21 11:53_

Stop making changes to `bool` xD

---

_@MichaReiser reviewed on 2025-02-21 11:53_

---

_@carljm approved on 2025-02-21 20:39_

Looks good to me! Needs a conflict resolved.

---

_Comment by @AlexWaygood on 2025-02-21 20:40_

> Looks good to me! Needs a conflict resolved.

I resolved it 1 minute prior to your review ;)

---

_@carljm reviewed on 2025-02-21 20:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2894 on 2025-02-21 20:43_

If e.g. `ModuleType` actually had a `def __bool__(self) -> Literal[True]` annotation, then we could rely on Liskov to say that all modules must be boolean true. But it doesn't (which is probably reasonable.

---

_Comment by @carljm on 2025-02-21 20:44_

Still looks good post-merge resolution 😆 

---

_@AlexWaygood reviewed on 2025-02-21 20:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2894 on 2025-02-21 20:44_

Agree

---

_Merged by @AlexWaygood on 2025-02-21 20:46_

---

_Closed by @AlexWaygood on 2025-02-21 20:46_

---

_Branch deleted on 2025-02-21 20:46_

---
