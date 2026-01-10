```yaml
number: 21466
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-11-15T00:49:53Z
updated_at: 2025-11-15T19:56:03Z
url: https://github.com/astral-sh/ruff/pull/21466
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-11-15 00:49_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-11-15 00:49_

---

_Label `ty` added by @github-actions[bot] on 2025-11-15 00:49_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-11-15 00:49_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-11-15 00:49_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-11-15 00:49_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-11-15 00:49_

---

_Closed by @AlexWaygood on 2025-11-15 00:52_

---

_Reopened by @AlexWaygood on 2025-11-15 00:52_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 00:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-15 17:05:22.546598272 +0000
+++ new-output.txt	2025-11-15 17:05:26.023644693 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19aa2)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19aa3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-15 00:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
urllib3 (https://github.com/urllib3/urllib3)
+ test/test_ssltransport.py:308:59: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `list[Unknown | socket] & ~AlwaysFalsy`
+ test/test_ssltransport.py:308:75: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `list[Unknown | socket] & ~AlwaysFalsy`
- Found 344 diagnostics
+ Found 346 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg/psycopg/waiting.py:266:17: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `tuple[int] | tuple[()]`
+ psycopg/psycopg/waiting.py:267:17: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `tuple[int] | tuple[()]`
- Found 633 diagnostics
+ Found 635 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/annotations.py:2323:1: error[invalid-assignment] Object of type `dict[Unknown | <class 'Attachment'> | <class 'bool'> | ... omitted 10 union elements, Unknown | NotImplementedType | tuple[(value: str, /) -> bool] | ... omitted 7 union elements]` is not assignable to `dict[Any, tuple[(...) -> Any, ...]]`
- Found 145 diagnostics
+ Found 146 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:541:48: error[unresolved-attribute] Module `stat` has no member `FILE_ATTRIBUTE_REPARSE_POINT`
- Found 1727 diagnostics
+ Found 1726 diagnostics

manticore (https://github.com/trailofbits/manticore)
+ manticore/platforms/linux.py:328:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:331:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:334:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:341:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:344:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:347:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:350:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:353:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
- Found 11085 diagnostics
+ Found 11093 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/command/build_py.py:391:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `str`
+ setuptools/_distutils/command/build_py.py:391:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `LiteralString`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/demos/excelRTDServer.py:239:15: error[call-non-callable] Object of type `_NotImplementedType` is not callable
+ com/win32com/demos/excelRTDServer.py:239:15: error[call-non-callable] Object of type `NotImplementedType` is not callable
- com/win32com/demos/excelRTDServer.py:278:15: error[call-non-callable] Object of type `_NotImplementedType` is not callable
+ com/win32com/demos/excelRTDServer.py:278:15: error[call-non-callable] Object of type `NotImplementedType` is not callable

scipy (https://github.com/scipy/scipy)
+ scipy/integrate/_ivp/rk.py:76:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `ndarray[@Todo, dtype[Any]]`
+ scipy/integrate/_ivp/rk.py:77:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `ndarray[@Todo, dtype[Any]]`
+ scipy/integrate/_ivp/rk.py:78:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `ndarray[@Todo, dtype[Any]]`
+ scipy/integrate/_ivp/rk.py:79:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `ndarray[@Todo, dtype[Any]]`
+ scipy/integrate/_ivp/rk.py:80:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `ndarray[@Todo, dtype[Any]]`
+ scipy/integrate/_ivp/rk.py:81:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `int`
+ scipy/integrate/_ivp/rk.py:82:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `int`
+ scipy/integrate/_ivp/rk.py:83:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `int`
+ scipy/optimize/_trustregion_constr/report.py:4:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `list[str]`
+ scipy/optimize/_trustregion_constr/report.py:5:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `list[int]`
+ scipy/optimize/_trustregion_constr/report.py:6:5: error[invalid-assignment] Object of type `NotImplementedType` is not assignable to `list[str]`
+ scipy/stats/tests/test_qmc.py:538:20: error[call-non-callable] Object of type `NotImplementedType` is not callable
+ scipy/stats/tests/test_qmc.py:543:24: error[call-non-callable] Object of type `NotImplementedType` is not callable
- Found 6696 diagnostics
+ Found 6709 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-15 14:22_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 14:28_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 12 | 0 | 0 |
| `invalid-raise` | 8 | 0 | 0 |
| `call-non-callable` | 2 | 0 | 2 |
| `invalid-argument-type` | 4 | 0 | 0 |
| `unresolved-attribute` | 0 | 1 | 0 |
| `unsupported-operator` | 0 | 0 | 1 |
| **Total** | **26** | **1** | **3** |

**[Full report with detailed diff](https://typeshedbot-sync-typeshed.ecosystem-663.pages.dev/diff)** ([timing results](https://typeshedbot-sync-typeshed.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-11-15 14:34_

Hmm, there's a regression here -- we're no longer emitting an error on this line, even though it's obviously incorrect: <https://github.com/mhammond/pywin32/blob/572e657c6c24415fc364d9e31bbe78c0dac6eb9c/com/win32com/demos/excelRTDServer.py#L239>.

(They meant to write `raise NotImplementedError("Subclass must implement")`, not `raise NotImplemented("Subclass must implement")`)

---

_Comment by @AlexWaygood on 2025-11-15 14:42_

Okay, I see the cause of the regression.

Prior to https://github.com/python/typeshed/pull/14966, `builtins._NotImplementedType` had a fictitious `__call__: None` annotation, but `types.NotImplementedType` did not. https://github.com/python/typeshed/pull/14966 and https://github.com/python/typeshed/pull/14971 got rid of the fiction, which is probably for the best: `NotImplementedType` does not actually have a `__call__ = None` attribute at runtime. But typeshed still has both versions of the type inheriting from `Any`, which means that now the `__call__` fiction has been removed, ty now thinks the builtin `NotImplemented` constant is callable, which is somewhat unfortunate.

Considering how much special-casing we have to hardcode for `NotImplemented` anyway, I think the inheritance from `Any` that typeshed gives us probably hurts us more than it helps us. The best thing to do here may well be to just override the MRO that typeshed gives us -- I'll try that out.

---

_@AlexWaygood reviewed on 2025-11-15 14:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:205 on 2025-11-15 14:58_

Ideally we'd give a helpful subdiagnostic telling them that they probably meant to call `NotImplementedError()` rather than `NotImplemented` (the two are very different, but are very often confused for each other). I'll pick that up as a followup, though -- it's nontrivial to implement.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-15 14:58_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-15 14:58_

---

_@AlexWaygood reviewed on 2025-11-15 14:59_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:224 on 2025-11-15 14:59_

(So that the autocompletion evaluation results are automatically updated as part of the next typeshed sync workflow)

---

_Comment by @AlexWaygood on 2025-11-15 15:06_

All the new diagnostics regarding `NotImplemented` now look like true positives to me

---

_Comment by @AlexWaygood on 2025-11-15 15:13_

(I'd love a quick once-over from someone else to check the changes I've pushed to this PR!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:480 on 2025-11-15 15:21_

hmm, mypy/pyright/pyrefly all do not emit a diagnostic here. And the fact that we reject this is the cause of many of the new scipy diagnostics. But it seems clearly unsound to me; I'd see the fact that we reject this as a feature rather than an incompatibility.

---

_@AlexWaygood reviewed on 2025-11-15 15:21_

---

_@AlexWaygood reviewed on 2025-11-15 15:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:480 on 2025-11-15 15:54_

And I think it's probably just because of the fiction typeshed tells about the MRO of `NotImplementedType`, rather than a deliberately implemented feature of any of those type checkers

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:205 on 2025-11-15 16:11_

Maybe file an issue rather than following up immediately, though? It's a nice diagnostic improvement, but I think e.g. improving Liskov is higher priority for beta.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:480 on 2025-11-15 16:12_

I agree.

---

_@carljm approved on 2025-11-15 16:13_

Thank you!

---

_@AlexWaygood reviewed on 2025-11-15 16:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:205 on 2025-11-15 16:28_

Grrr fine

---

_Merged by @AlexWaygood on 2025-11-15 17:12_

---

_Closed by @AlexWaygood on 2025-11-15 17:12_

---

_Branch deleted on 2025-11-15 17:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:205 on 2025-11-15 19:56_

Well, I couldn't resist doing _some_ of this now:
- https://github.com/astral-sh/ruff/pull/21475

But I opened an issue for the more fiddly part of this:

- https://github.com/astral-sh/ty/issues/1571

---

_@AlexWaygood reviewed on 2025-11-15 19:56_

---
