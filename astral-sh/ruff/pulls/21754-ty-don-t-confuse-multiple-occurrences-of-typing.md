```yaml
number: 21754
title: "[ty] Don't confuse multiple occurrences of `typing.Self` when binding bound methods"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/ty1713
created_at: 2025-12-02T16:00:05Z
updated_at: 2025-12-02T18:15:11Z
url: https://github.com/astral-sh/ruff/pull/21754
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Don't confuse multiple occurrences of `typing.Self` when binding bound methods

---

_@dcreager_

In the following example, there are two occurrences of `typing.Self`, one for `Foo.foo` and one for `Bar.bar`:

```py
from typing import Self, reveal_type

class Foo[T]:
    def foo(self: Self) -> T:
        raise NotImplementedError

class Bar:
    def bar(self: Self, x: Foo[Self]):
        # SHOULD BE: bound method Foo[Self@bar].foo() -> Self@bar
        # revealed: bound method Foo[Self@bar].foo() -> Foo[Self@bar]
        reveal_type(x.foo)

def f[U: Bar](x: Foo[U]):
    # revealed: bound method Foo[U@f].foo() -> U@f
    reveal_type(x.foo)
```

When accessing a bound method, we replace any occurrences of `Self` with the bound `self` type.

We were doing this correctly for the second reveal. We would first apply the specialization, getting `(self: Self@foo) -> U@F` as the signature of `x.foo`. We would then bind the `self` parameter, substituting `Self@foo` with `Foo[U@F]` as part of that. The return type was already specialized to `U@F`, so that substitution had no further affect on the type that we revealed.

In the first reveal, we would follow the same process, but we confused the two occurrences of `Self`. We would first apply the specialization, getting `(self: Self@foo) -> Self@bar` as the method signature. We would then try to bind the `self` parameter, substituting `Self@foo` with `Foo[Self@bar]`. However, because we didn't distinguish the two separate `Self`s, and applied the substitution to the return type as well as to the `self` parameter.

The fix is to track which particular `Self` we're trying to substitute when applying the type mapping.

Fixes https://github.com/astral-sh/ty/issues/1713

---

_Label `ty` added by @dcreager on 2025-12-02 16:00_

---

_Review requested from @carljm by @dcreager on 2025-12-02 16:00_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-02 16:00_

---

_Review requested from @sharkdp by @dcreager on 2025-12-02 16:00_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 16:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 16:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_raw/onion.py:417:9: error[invalid-assignment] Object of type `ReferenceType[Self@add_child]` is not assignable to attribute `__parent_ref` of type `Function[Unknown, CatalogOnionLayerImpl | None]`
- Found 286 diagnostics
+ Found 285 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 44 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/indexes/base.py:6464:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 3792 diagnostics
+ Found 3791 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@ibraheemdev approved on 2025-12-02 16:15_

Nice!

---

_Merged by @dcreager on 2025-12-02 18:15_

---

_Closed by @dcreager on 2025-12-02 18:15_

---

_Branch deleted on 2025-12-02 18:15_

---
