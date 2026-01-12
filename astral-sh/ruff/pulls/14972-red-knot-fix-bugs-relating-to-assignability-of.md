```yaml
number: 14972
title: "[red-knot] Fix bugs relating to assignability of dynamic `type[]` types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/type-unknown-assignability
created_at: 2024-12-14T16:17:14Z
updated_at: 2025-05-07T15:22:46Z
url: https://github.com/astral-sh/ruff/pull/14972
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Fix bugs relating to assignability of dynamic `type[]` types

---

_@AlexWaygood_

## Summary

- `type[Unknown]` and `type[Todo]` should be considered assignable to all `type[]` types, just like `type[Any]`. Currently we do not treat them as such.
- `type[Any]` should be considered assignable to instances of subclasses of `type`. Currently we only consider it assignable to `Instance("builtins.type")`, and do not consider it assignable to `Instance("abc.ABCMeta")` -- but `ABCMeta` is a subclass of `type`.

## Test Plan

New unit tests and mdtests added. They fail on `main`.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-14 16:17_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-14 16:17_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-14 16:17_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-14 16:17_

---

_@AlexWaygood reviewed on 2024-12-14 16:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_of/dynamic.md`:1 on 2024-12-14 16:18_

I renamed this file from `any.md` to `dynamic.md` since it now contains tests for `type[Unknown]` as well as `type[Any]`. They're not different enough to justify a separate test file for `type[Unknown]`, I don't think.

---

_@AlexWaygood reviewed on 2024-12-14 16:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:866 on 2024-12-14 16:19_

or should I use `is_assignable_to` here...? I suppose we currently consider all `Instance` types fully static, but that will no longer be the case after we fix https://github.com/astral-sh/ruff/issues/14774?

---

_Label `bug` added by @AlexWaygood on 2024-12-14 16:19_

---

_Comment by @github-actions[bot] on 2024-12-14 16:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/dynamic.md`:16 on 2024-12-14 19:31_

This is OK, but I feel like this would be easier to read and understand (for me, anyway):
```suggestion
def f(x: type[Any], y: type[str]):
    reveal_type(x)  # revealed: type[Any]
    # TODO: could be `<object.__repr__ type> & Any`
    reveal_type(x.__repr__)  # revealed: Any

    # type[str] and type[Any] are assignable to each other
    a: type[str] = x
    b: type[Any] = y
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/dynamic.md`:101 on 2024-12-14 19:33_

This test checks the opposite of what the docstring says. It checks that `type[Any]` and `type[Unknown]` are assignable to `ABCMeta`; it doesn't check the reverse.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:866 on 2024-12-14 19:35_

Probably should use `is_assignable`, as it's correct here, and will be more robust in future when `Type::Instance` is not necessarily fully-static.

---

_@carljm approved on 2024-12-14 19:36_

Thank you!

---

_@AlexWaygood reviewed on 2024-12-15 01:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_of/dynamic.md`:16 on 2024-12-15 01:02_

ah yeah, that's much better. Thanks!

---

_Merged by @AlexWaygood on 2024-12-15 01:15_

---

_Closed by @AlexWaygood on 2024-12-15 01:15_

---

_Branch deleted on 2024-12-15 01:15_

---
