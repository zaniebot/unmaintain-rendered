```yaml
number: 20031
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
created_at: 2025-08-21T19:15:24Z
updated_at: 2025-08-21T21:40:22Z
url: https://github.com/astral-sh/ruff/pull/20031
synced_at: 2026-01-12T15:56:53Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2025-08-21 19:15_

---

_Review requested from @carljm by @github-actions[bot] on 2025-08-21 19:15_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-08-21 19:15_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-08-21 19:15_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-08-21 19:15_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-08-21 19:15_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-21 19:16_

---

_Comment by @github-actions[bot] on 2025-08-21 19:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-21 21:30:47.762457018 +0000
+++ new-output.txt	2025-08-21 21:30:50.262467550 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(bc7d)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(bcb1)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @AlexWaygood on 2025-08-21 19:22_

The big change in this update is https://github.com/python/typeshed/pull/14583, which changes the definition of `Generic` from `Generic: _SpecialForm` to `Generic: type[_Generic]`, making it more accurate with respect to the definition of `Generic` at runtime but requiring a couple of changes from us in `infer.rs` and `special_form.rs`.

---

_Comment by @github-actions[bot] on 2025-08-21 19:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- parso/tree.py:1:33: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
- parso/tree.py:136:6: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ parso/tree.py:1:33: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.
+ parso/tree.py:136:6: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.
- parso/tree.py:144:6: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ parso/tree.py:144:6: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.

comtypes (https://github.com/enthought/comtypes)
- comtypes/test/__init__.py:137:21: warning[deprecated] The function `makeSuite` is deprecated: Deprecated in Python 3.11; removal scheduled for Python 3.13
- comtypes/test/__init__.py:207:24: warning[deprecated] The function `makeSuite` is deprecated: Deprecated in Python 3.11; removal scheduled for Python 3.13
- Found 401 diagnostics
+ Found 399 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain.py:6354:25: warning[deprecated] The function `warn` is deprecated: Deprecated; use warning() instead.
+ dulwich/porcelain.py:6354:25: warning[deprecated] The function `warn` is deprecated: Deprecated since Python 3.3. Use `warning()` instead.

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/codec_options.py:63:10: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
- bson/codec_options.py:82:10: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ bson/codec_options.py:63:10: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.
+ bson/codec_options.py:82:10: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.

mkosi (https://github.com/systemd/mkosi)
- mkosi/documentation.py:32:29: warning[deprecated] The function `warn` is deprecated: Deprecated; use warning() instead.
+ mkosi/documentation.py:32:29: warning[deprecated] The function `warn` is deprecated: Deprecated since Python 3.3. Use `warning()` instead.

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/__init__.py:4:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 181 diagnostics
+ Found 180 diagnostics

vision (https://github.com/pytorch/vision)
- test/datasets_utils.py:1043:9: error[invalid-assignment] Object of type `LiteralString` is not assignable to `tuple[str, ...]`
+ test/datasets_utils.py:1043:9: error[invalid-assignment] Object of type `Literal["abcdefghijklmnopqrstuvwxyz"]` is not assignable to `tuple[str, ...]`

psycopg (https://github.com/psycopg/psycopg)
+ tests/types/test_uuid.py:62:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 641 diagnostics
+ Found 642 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/series_mapping.py:21:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/series_mapping.py:26:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/series_mapping.py:32:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1779 diagnostics
+ Found 1782 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- misc/python/materialize/parallel_workload/action.py:80:46: warning[deprecated] The function `getName` is deprecated: Use the name attribute instead
+ misc/python/materialize/parallel_workload/action.py:80:46: warning[deprecated] The function `getName` is deprecated: Deprecated since Python 3.10. Read the `name` attribute instead.
- misc/python/materialize/parallel_workload/action.py:1741:40: warning[deprecated] The function `getName` is deprecated: Use the name attribute instead
+ misc/python/materialize/parallel_workload/action.py:1741:40: warning[deprecated] The function `getName` is deprecated: Deprecated since Python 3.10. Read the `name` attribute instead.
- misc/python/materialize/parallel_workload/executor.py:109:50: warning[deprecated] The function `getName` is deprecated: Use the name attribute instead
+ misc/python/materialize/parallel_workload/executor.py:109:50: warning[deprecated] The function `getName` is deprecated: Deprecated since Python 3.10. Read the `name` attribute instead.
- misc/python/materialize/parallel_workload/worker.py:131:62: warning[deprecated] The function `getName` is deprecated: Use the name attribute instead
+ misc/python/materialize/parallel_workload/worker.py:131:62: warning[deprecated] The function `getName` is deprecated: Deprecated since Python 3.10. Read the `name` attribute instead.
- test/rtr-combined/mzcompose.py:67:54: warning[deprecated] The function `getName` is deprecated: Use the name attribute instead
+ test/rtr-combined/mzcompose.py:67:54: warning[deprecated] The function `getName` is deprecated: Deprecated since Python 3.10. Read the `name` attribute instead.

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/results.py:142:9: error[invalid-assignment] Object of type `(@Todo(Inference of subscript on special form) & Block) | (UUID & Block) | (Path & Block)` is not assignable to `WritableFileSystem`
- src/prefect/results.py:179:9: error[invalid-assignment] Object of type `(@Todo(Inference of subscript on special form) & Block) | (UUID & Block) | (Path & Block)` is not assignable to `WritableFileSystem`
- Found 2951 diagnostics
+ Found 2949 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/build.py:534:10: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ mesonbuild/build.py:534:10: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.
- mesonbuild/linkers/linkers.py:126:10: warning[deprecated] The class `abstractproperty` is deprecated: Use 'property' with 'abstractmethod' instead
+ mesonbuild/linkers/linkers.py:126:10: warning[deprecated] The class `abstractproperty` is deprecated: Deprecated since Python 3.3. Use `@property` stacked on top of `@abstractmethod` instead.

sympy (https://github.com/sympy/sympy)
- sympy/testing/runtests.py:1176:28: warning[deprecated] The class `Str` is deprecated: Replaced by ast.Constant; removed in Python 3.14
+ sympy/testing/runtests.py:1176:28: warning[deprecated] The class `Str` is deprecated: Removed in Python 3.14. Use `ast.Constant` instead.
- sympy/testing/runtests.py:1195:33: warning[deprecated] The class `Str` is deprecated: Replaced by ast.Constant; removed in Python 3.14
+ sympy/testing/runtests.py:1195:33: warning[deprecated] The class `Str` is deprecated: Removed in Python 3.14. Use `ast.Constant` instead.

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-21 19:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeshedbot%2Fsync-typeshed?runnerMode=Instrumentation)

### Merging #20031 will **degrade performances by 5.32%**

<sub>Comparing <code>typeshedbot/sync-typeshed</code> (123e5b5) with <code>main</code> (f82025d)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_micro[many_string_assignments] `` | 67.9 ms | 71.7 ms | -5.32% |


---

_Comment by @AlexWaygood on 2025-08-21 19:54_

it's still not totally clear to me why https://github.com/astral-sh/ruff/pull/20031/commits/daeb839f59324d11db156b6eb19edc2c580441b2 leads to that much of a slowdown on the `many_string_assignments` microbenchmark... but it _is_ a microbenchmark rather than realistic code, and I can't find any obvious ways of speeding it up (from playing around with it locally)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/special_form.rs`:159 on 2025-08-21 20:41_

I do see that a number of our benchmarks (including tomllib) have a 1% regression from this PR, so it's not _just_ the one micro-benchmark (although clearly it's much worse there.)

I suspect the reason for the regression is that there are some cases where we fall back to `KnownClass::Type` instead of to `KnownClass::SpecialForm` now, and that's more expensive?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:4685 on 2025-08-21 20:42_

We now call `SpecialFormType::try_from_file_and_name` on literally every single annotated assignment to a Name target -- that's a lot of annotated assignments. `try_from_file_and_name` should usually be just a string compare, but that can add up? Is there any way we could filter out more of the cases here and see if that helps? Like, it now either needs to be a `KnownClass::SpecialForm` or a `SubclassOf` type?

---

_@carljm approved on 2025-08-21 20:43_

If we can't figure out the regression, we can land this, though it does seem unfortunate to eat ~1% based on a typeshed change that doesn't really offer much user value.

---

_Comment by @AlexWaygood on 2025-08-21 20:48_

> If we can't figure out the regression, we can land this, though it does seem unfortunate to eat ~1% based on a typeshed change that doesn't really offer much user value.

I think it will offer user value once we properly type check calls to `isinstance()` and `issubclass()` (currently we don't because they're annotated with recursive type aliases). It's not uncommon to pass `Generic` as the second argument to `isinstance()` or `issubclass()` -- see https://github.com/python/typeshed/pull/14583#issuecomment-3202421558 -- and this change means that we now understand that the symbol `Generic` is an instance of `type`, which means we'll continue to accept it being passed as the second argument to those functions even after we start properly type-checking them.

---

_@AlexWaygood reviewed on 2025-08-21 20:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4685 on 2025-08-21 20:52_

I tried something like that locally and it didn't seem to help much, but let's see what codspeed thinks about https://github.com/astral-sh/ruff/pull/20031/commits/59f1c72e4c513bb499560894199277a1f4d75c56

---

_@AlexWaygood reviewed on 2025-08-21 21:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4685 on 2025-08-21 21:28_

yeah, it's not helping at all, and may even be hurting slightly, so I'll revert it since it means more code.

---

_Merged by @AlexWaygood on 2025-08-21 21:32_

---

_Closed by @AlexWaygood on 2025-08-21 21:32_

---

_Branch deleted on 2025-08-21 21:32_

---
