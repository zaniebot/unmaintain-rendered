```yaml
number: 21762
title: "[ty] implement goto-type and display types in hovers for the remaining non-expression identifiers"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/stmt-types
created_at: 2025-12-02T19:45:00Z
updated_at: 2025-12-03T22:34:27Z
url: https://github.com/astral-sh/ruff/pull/21762
synced_at: 2026-01-12T15:57:32Z
```

# [ty] implement goto-type and display types in hovers for the remaining non-expression identifiers

---

_@Gankra_

## Summary

The basic idea here is we already allow passing `Identifier` to several APIs that can query "Expressions", so really all that's missing is trying to do it... and then teaching the inference engine to actually compute and record those types. Unfortunately, the inference changes are beyond me. They're conceptually trivial for `nonlocal x` and `global x` but the actual mechanics of refactoring the code is something I'm not gonna attempt.

For patterns I think we're much further away, as we infer `@TODO` for pattern captures. So even if we wired it up, it wouldnt actually be *useful*.

For TypeParams I think there might be no reason at all to implement this functionality, because, what will be show? The literal identifier you're hovering? I guess? (Edit: Alex begs to differ below)

* Fixes https://github.com/astral-sh/ty/issues/144

## Test Plan

Existing tests cover this, as you can see me churn them all to Unknown with the half implementation.


---

_Label `server` added by @Gankra on 2025-12-02 19:45_

---

_Label `ty` added by @Gankra on 2025-12-02 19:45_

---

_Comment by @Gankra on 2025-12-02 19:45_

I figured I'd put this up in case anyone ever gets inspired and wants to do this, but I have no intention of finishing this feature.

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 19:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 19:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 44 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-02 19:50_

> For TypeParams I think there might be no reason at all to implement this functionality, because, what will be show? The literal identifier you're hovering? I guess?

We could show the inferred variance of the type parameter, the scope to which it is bound (might be a function/method or a class), and the type parameter's upper bound/constraints, if it has any

---

_Closed by @Gankra on 2025-12-03 22:34_

---
