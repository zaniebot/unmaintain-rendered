```yaml
number: 20841
title: "[ty] Do not consider function-like property for `Callable` type relations"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/fix-1342
created_at: 2025-10-13T11:15:54Z
updated_at: 2025-10-13T11:47:23Z
url: https://github.com/astral-sh/ruff/pull/20841
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Do not consider function-like property for `Callable` type relations

---

_Pull request opened by @sharkdp on 2025-10-13 11:15_

## Summary

closes https://github.com/astral-sh/ty/issues/1342

## Ecosystem analysis

This looks good:

```diff
bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/templates.py:112:1: error[invalid-assignment] Object of type `dict[str, () -> Unknown]` is not assignable to `dict[str, () -> Template]`
```

## Test Plan

Regression test

---

_Label `ty` added by @sharkdp on 2025-10-13 11:15_

---

_Review requested from @carljm by @sharkdp on 2025-10-13 11:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-13 11:15_

---

_Review requested from @dcreager by @sharkdp on 2025-10-13 11:15_

---

_Converted to draft by @sharkdp on 2025-10-13 11:15_

---

_Renamed from "[ty] Do not function-like property for `Callable` type relations" to "[ty] Do not consider function-like property for `Callable` type relations" by @sharkdp on 2025-10-13 11:16_

---

_Comment by @github-actions[bot] on 2025-10-13 11:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 11:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 51 diagnostics
+ Found 52 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/templates.py:112:1: error[invalid-assignment] Object of type `dict[str, () -> Unknown]` is not assignable to `dict[str, () -> Template]`
- Found 465 diagnostics
+ Found 464 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-10-13 11:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1237 on 2025-10-13 11:26_

To be honest, I don't understand the reasoning behind these tests at all (I wrote them myself).


---

_Marked ready for review by @sharkdp on 2025-10-13 11:27_

---

_@AlexWaygood reviewed on 2025-10-13 11:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:1237 on 2025-10-13 11:39_

> Are there any important reasons why we should _not_ consider function-like callable types subtypes of equivalent non-function like callable types?

We should consider function-like `Callable`s subtypes of non-function-like `Callable`s. But I don't think we should ever consider non-function-like `Callable`s subtypes of function-like `Callable`s. I think that's what the logic on `main` is _trying_ to accomplish?

Non-function-like `Callable`s cannot be subtypes of function-like `Callable`s, because function-like `Callable`s have attributes from `FunctionType` available to them, such as `__name__`, `__qualname__` and `__kwdefaults__`. These aren't available on non-function-like `Callable`s.

This issue makes me wonder if we should reconsider having the concept of "function-like `Callable`s" in the first place -- an alternative way of representing this concept would just be to use `types.FunctionType & <some Callable type>`

---

_Converted to draft by @sharkdp on 2025-10-13 11:44_

---

_Closed by @sharkdp on 2025-10-13 11:47_

---
