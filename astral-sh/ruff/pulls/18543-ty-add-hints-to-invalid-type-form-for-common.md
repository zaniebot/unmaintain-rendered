```yaml
number: 18543
title: "[ty] Add hints to `invalid-type-form` for common mistakes"
type: pull_request
state: merged
author: benbaror
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: hints-for-invalid-type-form
created_at: 2025-06-08T06:05:27Z
updated_at: 2025-06-11T10:44:27Z
url: https://github.com/astral-sh/ruff/pull/18543
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Add hints to `invalid-type-form` for common mistakes

---

_Pull request opened by @benbaror on 2025-06-08 06:05_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves  https://github.com/astral-sh/ty/issues/593

1. Added a hint when using [...] instead of list[]
2. Emit diagnostic and hint when using (...) instead of tuple[...]

## Test Plan

Added md tests and snapshot

---

_Review requested from @carljm by @benbaror on 2025-06-08 06:05_

---

_Review requested from @AlexWaygood by @benbaror on 2025-06-08 06:05_

---

_Review requested from @sharkdp by @benbaror on 2025-06-08 06:05_

---

_Review requested from @dcreager by @benbaror on 2025-06-08 06:05_

---

_@AlexWaygood reviewed on 2025-06-08 11:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8575 on 2025-06-08 11:07_

I'd lean towards _only_ adding this hint if the list has 1 item in the slice. If it's `[int]`, it's very likely that the user wanted `list[int]`, but if it's something like `[int, str]`, it feels to me just as likely that the user wanted `tuple[int, str]`.

It would also be great if we could say "Did you mean `list[int]`?" in the suggestion if it was `[int]`, rather than just "Did you mean to use `list[...]`?"!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/invalid.md`:98 on 2025-06-08 11:11_

Enabling snapshots for this subsection creates a snapshot that's sort-of overwhelmingly huge 😆 I'm inclined to add a new subsection to the `## Diagnostics for common errors` section below, enable snapshots for that new subsection, and add duplicate tests to that section for the nodes where we offer tailored hints. It doesn't matter too much if some nodes are tested twice, once without snapshots and once with snapshots!

---

_@AlexWaygood reviewed on 2025-06-08 11:11_

---

_@AlexWaygood reviewed on 2025-06-08 11:11_

Thank you -- this looks on the right track! 😃

---

_Comment by @github-actions[bot] on 2025-06-08 11:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
+ error[invalid-type-form] src/trio/_core/_generated_io_kqueue.py:62:51: List literals are not allowed in this context in a type expression
+ error[invalid-type-form] src/trio/_core/_io_kqueue.py:150:30: List literals are not allowed in this context in a type expression
+ error[invalid-type-form] src/trio/_core/_io_windows.py:931:29: List literals are not allowed in this context in a type expression
- Found 1064 diagnostics
+ Found 1067 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ error[invalid-type-form] cki_lib/footer.py:75:50: List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
+ error[invalid-type-form] cki_lib/inttests/remote_responses.py:109:35: List literals are not allowed in this context in a type expression: Did you mean `list[PreparedRequest]`?
- Found 162 diagnostics
+ Found 164 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ error[invalid-type-form] pwndbg/aglib/disasm/riscv.py:121:54: List literals are not allowed in this context in a type expression: Did you mean `tuple[PwndbgInstruction, Emulator]`?
- Found 2317 diagnostics
+ Found 2318 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[possibly-unbound-implicit-call] static_frame/test/typing/test_index_hierarchy.py:17:22: Method `__getitem__` of type `Unknown | <class 'Index'>` is possibly unbound
- warning[possibly-unbound-implicit-call] static_frame/test/typing/test_index_hierarchy.py:17:35: Method `__getitem__` of type `Unknown | <class 'Index'>` is possibly unbound
- warning[possibly-unbound-implicit-call] static_frame/test/typing/test_index_hierarchy.py:20:22: Method `__getitem__` of type `Unknown | <class 'Index'>` is possibly unbound
- warning[possibly-unbound-implicit-call] static_frame/test/typing/test_index_hierarchy.py:20:35: Method `__getitem__` of type `Unknown | <class 'Index'>` is possibly unbound
- error[invalid-type-form] static_frame/test/unit/test_type_clinic.py:455:21: List literals are not allowed in this context in a type expression
+ error[invalid-type-form] static_frame/test/unit/test_type_clinic.py:455:21: List literals are not allowed in this context in a type expression: Did you mean `tuple[int, int]`?
- Found 1925 diagnostics
+ Found 1921 diagnostics

mypy (https://github.com/python/mypy)
+ error[invalid-type-form] mypy/typeshed/stdlib/importlib/resources/_common.pyi:19:28: List literals are not allowed in this context in a type expression: Did you mean `list[Unknown | None]`?
+ error[invalid-type-form] mypy/typeshed/stdlib/importlib/resources/_common.pyi:20:23: List literals are not allowed in this context in a type expression: Did you mean `tuple[Unknown | None, Unknown | None]`?
+ error[invalid-type-form] mypy/typeshed/stdlib/importlib/resources/_common.pyi:42:76: Boolean literals are not allowed in this context in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/importlib/resources/_functional.pyi:28:92: Boolean literals are not allowed in this context in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:416:53: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:416:74: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:540:37: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:540:49: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:540:64: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:557:48: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:604:48: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:606:69: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:611:30: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:616:30: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:710:41: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:710:49: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:723:41: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:736:46: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:750:34: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:750:39: Variable of type `TypeVar` is not allowed in a type expression
- error[unsupported-operator] mypy/typeshed/stdlib/typing.pyi:776:46: Operator `|` is unsupported between objects of type `TypeVar` and `None`
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:776:41: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:776:46: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:802:35: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:806:35: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypy/typeshed/stdlib/typing.pyi:808:35: Variable of type `TypeVar` is not allowed in a type expression
+ error[invalid-type-form] mypyc/test-data/fixtures/typing-full.pyi:100:38: Variable of type `object` is not allowed in a type expression
+ error[invalid-type-form] mypyc/test-data/fixtures/typing-full.pyi:100:43: Variable of type `object` is not allowed in a type expression
- Found 3183 diagnostics
+ Found 3209 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[invalid-type-form] ddtrace/internal/bytecode_injection/core.py:32:34: List literals are not allowed in this context in a type expression: Did you mean `list[InjectionContext]`?
- Found 6885 diagnostics
+ Found 6886 diagnostics

```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-06-08 11:29_

---

_@AlexWaygood approved on 2025-06-08 22:38_

Thank you, this is excellent! I pushed a refactor so that we don't calculate the hinted type so eagerly unless we know we're going to emit a diagnostic. That's quite important, as there's lots of situations where we need to infer types without emitting diagnostics (for example, if we're analzying third-party code in `site-packages` because we followed an import there from first-party code)

---

_Renamed from "[ty] `invalid-type-form` Hints and diagnostic for list and tuple" to "[ty] `invalid-type-form` Add hints to `invalid-type-form` for common mistakes" by @AlexWaygood on 2025-06-08 22:40_

---

_Renamed from "[ty] `invalid-type-form` Add hints to `invalid-type-form` for common mistakes" to "[ty] Add hints to `invalid-type-form` for common mistakes" by @AlexWaygood on 2025-06-08 22:40_

---

_Comment by @AlexWaygood on 2025-06-08 23:38_

That's a bit more primer output than I was expecting, but most of it is one of:
- Ty not understanding that `typing.TypeVar` in mypy's vendored version of typeshed is the same class as `typing.TypeVar` in ty's vendored version of typeshed. This isn't really a bug, just a weirdness that comes from type-checking all Python files in mypy's codebase with ty.
- Strange things happening in unreachable code branches, which mean that we don't understand that `Callable` is `Callable` or `Literal` is `Literal` in those branches. That's important because we only recognize int literals as being valid inside type expressions when they're nested inside `Literal` slices, and we only recognize list literals as being valid in type expressions if they appear as the first argument to `Callable`.

  We're planning on implementing a global solution soon so that we don't emit any false positives in unreachable code branches, so I'm not going to worry too much about this issue either right now.

---

_Merged by @AlexWaygood on 2025-06-08 23:40_

---

_Closed by @AlexWaygood on 2025-06-08 23:40_

---

_Label `diagnostics` added by @dhruvmanila on 2025-06-11 10:44_

---
