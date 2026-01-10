```yaml
number: 18121
title: "[ty] `type[…]` is always assignable to `type`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-312
created_at: 2025-05-15T14:06:12Z
updated_at: 2025-05-15T15:16:06Z
url: https://github.com/astral-sh/ruff/pull/18121
synced_at: 2026-01-10T18:51:01Z
```

# [ty] `type[…]` is always assignable to `type`

---

_Pull request opened by @sharkdp on 2025-05-15 14:06_

## Summary

Model that `type[C]` is always assignable to `type`, even if `C` is not fully static.

closes https://github.com/astral-sh/ty/issues/312

## Test Plan

* New Markdown tests
* Property tests

---

_Review requested from @carljm by @sharkdp on 2025-05-15 14:06_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-15 14:06_

---

_Review requested from @dcreager by @sharkdp on 2025-05-15 14:06_

---

_Label `ty` added by @sharkdp on 2025-05-15 14:06_

---

_Comment by @github-actions[bot] on 2025-05-15 14:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- error[no-matching-overload] src/trio/_path.py:77:16: No overload of function `__new__` matches arguments
- Found 1098 diagnostics
+ Found 1097 diagnostics

isort (https://github.com/pycqa/isort)
- error[invalid-argument-type] isort/exceptions.py:14:25: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[Unknown]`
- Found 55 diagnostics
+ Found 54 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- error[no-matching-overload] src/aiortc/sdp.py:233:67: No overload of function `__new__` matches arguments
- Found 130 diagnostics
+ Found 129 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-assignment] psycopg/psycopg/types/composite.py:303:9: Object of type `type[NamedTuple]` is not assignable to `((...) -> Any) | None`
- Found 1200 diagnostics
+ Found 1199 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[invalid-argument-type] mesonbuild/dependencies/detect.py:186:80: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[ExternalDependency]`
- error[invalid-argument-type] mesonbuild/dependencies/factory.py:93:65: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[ExtraFrameworkDependency]`
- error[invalid-argument-type] mesonbuild/dependencies/factory.py:94:60: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[PkgConfigDependency]`
- error[invalid-argument-type] mesonbuild/dependencies/factory.py:95:56: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[CMakeDependency] | Unknown`
- error[invalid-argument-type] mesonbuild/dependencies/factory.py:96:57: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[SystemDependency]`
- error[invalid-argument-type] mesonbuild/dependencies/factory.py:97:58: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[BuiltinDependency]`
- error[invalid-argument-type] mesonbuild/dependencies/factory.py:101:77: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[ConfigToolDependency]`
- Found 1459 diagnostics
+ Found 1452 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/core/container_util.py:518:23: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `type[IndexBase]`
+ warning[unused-ignore-comment] static_frame/core/metadata.py:123:67: Unused blanket `type: ignore` directive

sphinx (https://github.com/sphinx-doc/sphinx)
- error[no-matching-overload] sphinx/testing/path.py:192:51: No overload of function `__new__` matches arguments
- Found 677 diagnostics
+ Found 676 diagnostics

```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1526 on 2025-05-15 14:31_

I doubt it matters much in this situation, but I'd prefer to express this in a slightly more general way:

```suggestion
            (Type::SubclassOf(_), _)
                if KnownClass::Type.to_instance(db).is_assignable_to(db, target) =>
            {
                true
            }
```

---

_@AlexWaygood approved on 2025-05-15 14:31_

Thank you!

---

_Comment by @AlexWaygood on 2025-05-15 14:31_

> ## Test Plan
> 
> New Markdown tests

Could you also double-check the property tests still pass?

---

_Comment by @sharkdp on 2025-05-15 15:08_

> Could you also double-check the property tests still pass?

Oh, I failed to mention it, but I did actually run them :smile: 

---

_@sharkdp reviewed on 2025-05-15 15:11_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1526 on 2025-05-15 15:11_

> I doubt it matters much in this situation

Turns out it *does* matter. We have a few ecosystem results now. I think they look correct.

---

_Merged by @sharkdp on 2025-05-15 15:13_

---

_Closed by @sharkdp on 2025-05-15 15:13_

---

_Branch deleted on 2025-05-15 15:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1526 on 2025-05-15 15:16_

nice!

---

_@AlexWaygood reviewed on 2025-05-15 15:16_

---
