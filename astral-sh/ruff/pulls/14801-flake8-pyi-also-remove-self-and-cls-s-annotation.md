```yaml
number: 14801
title: "[`flake8-pyi`] Also remove `self` and `cls`'s annotation (`PYI034`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: PYI034
created_at: 2024-12-06T00:13:48Z
updated_at: 2024-12-09T15:13:14Z
url: https://github.com/astral-sh/ruff/pull/14801
synced_at: 2026-01-10T20:42:27Z
```

# [`flake8-pyi`] Also remove `self` and `cls`'s annotation (`PYI034`)

---

_Pull request opened by @InSyncWithFoo on 2024-12-06 00:13_

## Summary

Resolves #14421.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-06 00:13_

---

_Comment by @github-actions[bot] on 2024-12-06 00:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-12-06 12:32_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:180 on 2024-12-06 12:32_

I think even in stub contexts, this could lead to false positives where we say conclude that a class is not generic even though it actually is, and then incorrectly apply an autofix based on that conclusion. That's because the `is_old_style_typevar_like` function doesn't recognise typevars declared in other files as being typevars: I don't think it would understand `typing.AnyStr`, for example. It's pretty rare in typeshed for stubs to use typevars imported from other files (other than `AnyStr`), but I've seen it done much more in other stubs. I definitely don't think we can safely assume that a symbol isn't a typevar just because it's been imported from another module.

Rather than `is_old_style_typevar_like` and `is_generic` functions, I think we actually need `could_be_old_style_typevar_like` and `could_be_generic` functions. `could_be_old_style_typevar_like` would return `true` if it can trace the binding to a TypeVar declaration in the same module, but it would also return `true` if the symbol comes from any other module. `could_be_generic` would have exactly the same implementation you have now, but it would use `could_be_old_style_typevar_like` instead of `is_typevar_like`.

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:224 on 2024-12-06 17:11_

this should return `true` because the binding _might_ refer to a TypeVar if there are no bindings (it could be imported via a `*` import) or multiple bindings

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:220 on 2024-12-06 17:12_

What about something like this?

```py
# a.pyi
from typing import TypeVar

T = TypeVar("T")
```

```py
# b.pyi
import a

class Foo:
    def __enter__(self: a.T) -> a.T: ...
```

I think you need to handle attribute expressions here as well

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:181 on 2024-12-06 17:14_

```suggestion
/// A class is considered generic if at least one of its direct bases
/// is subscripted with a `TypeVar`-like,
/// or if it is defined using PEP 695 syntax.
/// Therefore, a class *might* be generic if it uses PEP-695 syntax
/// or at least one of its direct bases is a subscript expression that
/// is subscripted with an object that *might* be a `TypeVar`-like.
```

---

_@AlexWaygood reviewed on 2024-12-06 17:15_

---

_@InSyncWithFoo reviewed on 2024-12-06 17:48_

---

_Review comment by @InSyncWithFoo on `crates/ruff_python_semantic/src/analyze/class.rs`:224 on 2024-12-06 17:48_

@AlexWaygood Changing this to `true` messes up the tests. I'm not sure why.

---

_Label `bug` added by @dylwil3 on 2024-12-06 23:00_

---

_Label `fixes` added by @dylwil3 on 2024-12-06 23:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-09 09:01_

---

_@AlexWaygood approved on 2024-12-09 14:56_

---

_Merged by @AlexWaygood on 2024-12-09 14:59_

---

_Closed by @AlexWaygood on 2024-12-09 14:59_

---

_Branch deleted on 2024-12-09 15:13_

---
