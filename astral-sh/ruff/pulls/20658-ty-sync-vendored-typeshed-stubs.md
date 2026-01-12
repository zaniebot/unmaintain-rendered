```yaml
number: 20658
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-10-01T00:41:56Z
updated_at: 2025-10-01T08:11:50Z
url: https://github.com/astral-sh/ruff/pull/20658
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-10-01 00:41_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-10-01 00:41_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-10-01 00:41_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-10-01 00:41_

---

_Label `ty` added by @github-actions[bot] on 2025-10-01 00:41_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-10-01 00:41_

---

_Closed by @sharkdp on 2025-10-01 05:15_

---

_Reopened by @sharkdp on 2025-10-01 05:15_

---

_Comment by @github-actions[bot] on 2025-10-01 05:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-01 07:48:06.888006507 +0000
+++ new-output.txt	2025-10-01 07:48:10.058006273 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(15c2d)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(15c32)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-01 05:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/commands/menu.py:63:40: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- tanjun/commands/message.py:62:34: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- tanjun/commands/slash.py:88:56: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- Found 120 diagnostics
+ Found 117 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/_specs.pyi:72:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `object` and `object`
- src/bokeh/_specs.pyi:72:25: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- src/bokeh/_specs.pyi:72:49: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- src/bokeh/core/property/visual.pyi:43:38: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- src/bokeh/core/property/visual.pyi:43:54: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- src/bokeh/core/property/visual.pyi:43:73: error[non-subscriptable] Cannot subscript object of type `UnionType` with no `__getitem__` method
- Found 466 diagnostics
+ Found 462 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/testing/tests/diagnose_imports.py:153:58: error[invalid-argument-type] Argument to function `__import__` is incorrect: Expected `Sequence[str]`, found `Unknown | None`
- Found 13913 diagnostics
+ Found 13912 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-10-01 07:06_

Looks like [this typeshed PR](https://github.com/python/typeshed/pull/14789) is causing us problems. Looking into it.

---

_Comment by @sharkdp on 2025-10-01 07:12_

Oh, and [this](https://github.com/python/typeshed/pull/14786/files) is also causing problems.

---

_@sharkdp reviewed on 2025-10-01 07:40_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/replace.md`:21 on 2025-10-01 07:40_

https://github.com/python/typeshed/pull/14786 changed the signature of `copy.replace` from
```py
_SR = TypeVar("_SR", bound=_SupportsReplace)

def replace(obj: _SR, /, **changes: Any) -> _SR: ...
```
to
```py
def replace(obj: _SupportsReplace[_RT_co], /, **changes: Any) -> _RT_co: ...
```
which our generics solver does not support yet :cry:.

I don't think it's important enough to implement a workaround for, though.

---

_Review request for @AlexWaygood removed by @sharkdp on 2025-10-01 08:04_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-01 08:04_

---

_@AlexWaygood reviewed on 2025-10-01 08:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/replace.md`:21 on 2025-10-01 08:07_

Agreed 

---

_@AlexWaygood approved on 2025-10-01 08:09_

Thank you!

---

_Merged by @sharkdp on 2025-10-01 08:11_

---

_Closed by @sharkdp on 2025-10-01 08:11_

---

_Branch deleted on 2025-10-01 08:11_

---
