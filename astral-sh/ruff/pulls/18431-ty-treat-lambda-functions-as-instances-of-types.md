```yaml
number: 18431
title: "[ty] Treat lambda functions as instances of types.FunctionType"
type: pull_request
state: merged
author: lipefree
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: lambda-as-function
created_at: 2025-06-02T14:55:24Z
updated_at: 2025-06-11T10:50:59Z
url: https://github.com/astral-sh/ruff/pull/18431
synced_at: 2026-01-12T15:56:18Z
```

# [ty] Treat lambda functions as instances of types.FunctionType

---

_@lipefree_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes https://github.com/astral-sh/ty/issues/567. 

Since `lambda` functions are instance of `types.FunctionType`, we should be able to access to attribute such as `__code__`, `__name__` and more. With this PR, we will have the following type and no more errors : 

```py
from typing import reveal_type

x = lambda y: y

reveal_type(x.__code__)  # revealed: CodeType
```

## Test Plan

<!-- How was it tested? -->

I have added mdtests, but since I am not familiar with attributes of `types.FunctionType`, my tests may be either too redundant or not exhaustive enough.


---

_Review requested from @carljm by @lipefree on 2025-06-02 14:55_

---

_Review requested from @AlexWaygood by @lipefree on 2025-06-02 14:55_

---

_Review requested from @sharkdp by @lipefree on 2025-06-02 14:55_

---

_Review requested from @dcreager by @lipefree on 2025-06-02 14:55_

---

_Label `ty` added by @AlexWaygood on 2025-06-02 14:56_

---

_Renamed from "[ty] Treat lambda functions as types.FunctionType" to "[ty] Treat lambda functions as instances of types.FunctionType" by @AlexWaygood on 2025-06-02 14:56_

---

_Comment by @github-actions[bot] on 2025-06-02 14:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- error[unresolved-attribute] static_frame/core/util.py:571:5: Unresolved attribute `__name__` on type `(rhs, lhs, func=Any) -> Unknown`.
- Found 1944 diagnostics
+ Found 1943 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ error[invalid-assignment] src/bokeh/core/templates.py:112:1: Object of type `dict[str, () -> Unknown]` is not assignable to `dict[str, () -> Template]`
- Found 951 diagnostics
+ Found 952 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[unresolved-attribute] sphinx/ext/autodoc/preserve_defaults.py:28:16: Type `() -> Unknown` has no attribute `__name__`
- Found 657 diagnostics
+ Found 656 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:227:38: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:257:38: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:313:41: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:332:34: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:358:37: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
+ warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:227:38: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:257:38: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:313:41: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:332:34: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
+ warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:358:37: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
- error[unresolved-attribute] tests/tracer/test_writer.py:311:34: Type `(*args) -> Unknown` has no attribute `__get__`
- Found 6861 diagnostics
+ Found 6860 diagnostics

sympy (https://github.com/sympy/sympy)
- error[unsupported-operator] sympy/codegen/tests/test_applications.py:23:17: Operator `<` is not supported for types `Variable` and `Variable`
- error[unresolved-attribute] sympy/tensor/array/array_comprehension.py:342:58: Type `() -> Unknown` has no attribute `__name__`
- Found 18575 diagnostics
+ Found 18573 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-06-02 15:45_

> ```diff
> + error[invalid-assignment] src/bokeh/core/templates.py:112:1: Object of type `dict[str, () -> Unknown]` is not assignable to `dict[str, () -> Template]`
> ```

This error is occuring because `dict` is invariant: https://github.com/bokeh/bokeh/blob/7d6cedea470cb68f25afd126a29a45370c08f443/src/bokeh/core/templates.py#L112-L127. Bidirectional inference would fix this.

> ```diff
> + warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:227:38: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
> + warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:257:38: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
> + warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:313:41: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
> + warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:332:34: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
> + warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:358:37: Attribute `__name__` on type `((Any, /) -> bool) | ((_) -> Unknown)` is possibly unbound
> ```

These are because we do not infer `Callable` types created from `Callable` type expressions as being function-like. Whether or not we should do that is a complicated question that's discussed in other issues.

So all the primer hits here look good to me!

---

_@AlexWaygood approved on 2025-06-02 15:46_

Thank you!

---

_Merged by @AlexWaygood on 2025-06-02 15:46_

---

_Closed by @AlexWaygood on 2025-06-02 15:46_

---

_Label `bug` added by @dhruvmanila on 2025-06-11 10:50_

---
