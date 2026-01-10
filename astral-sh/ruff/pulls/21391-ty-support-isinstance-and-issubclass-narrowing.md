```yaml
number: 21391
title: "[ty] Support `isinstance()` and `issubclass()` narrowing when the second argument is a `typing.py` stdlib alias"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/stdlib-alias-narrowing
created_at: 2025-11-11T19:56:03Z
updated_at: 2025-11-11T21:09:27Z
url: https://github.com/astral-sh/ruff/pull/21391
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Support `isinstance()` and `issubclass()` narrowing when the second argument is a `typing.py` stdlib alias

---

_Pull request opened by @AlexWaygood on 2025-11-11 19:56_

## Summary

A followup to https://github.com/astral-sh/ruff/pull/21386

## Test Plan

New mdtests added


---

_Label `ty` added by @AlexWaygood on 2025-11-11 19:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 19:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 19:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- kornia/utils/draw.py:379:38: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | list[Unknown]`
- kornia/utils/draw.py:379:54: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `Unknown | list[Unknown]`
- kornia/utils/draw.py:379:71: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Unknown | list[Unknown]`
- Found 762 diagnostics
+ Found 759 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/appsec/_iast/_ast/visitor.py:785:32: warning[possibly-missing-attribute] Attribute `elts` may be missing on object of type `(expr & Top[list[Unknown]] & ~Subscript) | (Tuple & ~Subscript)`
- ddtrace/appsec/_iast/_ast/visitor.py:785:32: error[unresolved-attribute] Object of type `expr & ~Subscript` has no attribute `elts`
- ddtrace/appsec/_iast/_ast/visitor.py:787:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 8226 diagnostics
+ Found 8225 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/matrices/sparse.py:143:46: error[call-non-callable] Object of type `None` is not callable
- sympy/matrices/sparse.py:174:56: error[not-iterable] Object of type `(Unknown & ~Top[dict[Unknown, Unknown]] & ~Dict) | None` may not be iterable
+ sympy/matrices/sparse.py:174:56: error[not-iterable] Object of type `(Unknown & ~(() -> object) & ~Top[dict[Unknown, Unknown]] & ~Dict) | None` may not be iterable
- sympy/matrices/sparse.py:184:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Top[dict[Unknown, Unknown]] & ~Dict) | None`
+ sympy/matrices/sparse.py:184:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~(() -> object) & ~Top[dict[Unknown, Unknown]] & ~Dict) | None`
- Found 14444 diagnostics
+ Found 14443 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-11-11 20:02_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-11 20:02_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-11 20:02_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-11 20:02_

---

_Comment by @AlexWaygood on 2025-11-11 20:06_

Not a huge ecosystem impact, but I think this is worth it for consistency as much as anything else.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:233 on 2025-11-11 20:08_

This comment suggests this is a ty limitation, but at runtime `issubclass(..., typing.Callable)` just raises `TypeError`, so I don't think this is something we should support.

We should probably revise your previous PR so that we emit a diagnostic on that again?

---

_@carljm approved on 2025-11-11 20:08_

Love it!

---

_@AlexWaygood reviewed on 2025-11-11 20:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:233 on 2025-11-11 20:09_

Oh it does? Huh, I'll fix that, thanks

---

_@AlexWaygood reviewed on 2025-11-11 20:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:233 on 2025-11-11 20:10_

> at runtime `issubclass(..., typing.Callable)` just raises `TypeError`

not on my machine?

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import typing as t
>>> issubclass(int, t.Callable)
False
>>> issubclass(type(lambda: 42), t.Callable)
True
```

---

_@AlexWaygood reviewed on 2025-11-11 20:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:233 on 2025-11-11 20:12_

Did you literally pass in a `...` for the first argument? If so then the runtime was probably complaining about your first argument not being an instance of `type` rather than anything about the second argument 😄

---

_@carljm reviewed on 2025-11-11 20:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:233 on 2025-11-11 20:49_

Yeah my mistake, sorry! 

---

_@carljm reviewed on 2025-11-11 20:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:233 on 2025-11-11 20:49_

Looks good as is. I guess in principle we could have a protocol as the meta-type of Callable...

---

_Merged by @AlexWaygood on 2025-11-11 21:09_

---

_Closed by @AlexWaygood on 2025-11-11 21:09_

---

_Branch deleted on 2025-11-11 21:09_

---
