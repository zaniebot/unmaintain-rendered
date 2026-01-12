```yaml
number: 18098
title: "[ty] Include synthesized arguments in displayed counts for `too-many-positional-arguments`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ty-too-many-positional-arguments
created_at: 2025-05-14T16:50:38Z
updated_at: 2025-05-15T07:40:53Z
url: https://github.com/astral-sh/ruff/pull/18098
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Include synthesized arguments in displayed counts for `too-many-positional-arguments`

---

_@InSyncWithFoo_

## Summary

Resolves [#290](https://github.com/astral-sh/ty/issues/290).

All arguments, synthesized or not, are now accounted for in `too-many-positional-arguments`'s error message.

For example, consider this example:

```python
class C:
	def foo(self): ...

C().foo(1)  # !!!
```

Previously, ty would say:

> Too many positional arguments to bound method foo: expected 0, got 1

After this change, it will say:

> Too many positional arguments to bound method foo: expected 1, got 2

This is what Python itself does too:

```text
Traceback (most recent call last):
  File "<python-input-0>", line 3, in <module>
    C().foo()
    ~~~~~~~^^
TypeError: C.foo() takes 0 positional arguments but 1 was given
```

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-14 16:50_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-14 16:50_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-14 16:50_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-14 16:50_

---

_Label `ty` added by @AlexWaygood on 2025-05-14 16:52_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-14 16:52_

---

_Closed by @carljm on 2025-05-15 02:42_

---

_Reopened by @carljm on 2025-05-15 02:42_

---

_@carljm approved on 2025-05-15 02:42_

Looks good, thank you!

---

_Comment by @github-actions[bot] on 2025-05-15 02:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- error[too-many-positional-arguments] src/attr/_make.py:3063:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/_make.py:3063:26: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:125:33: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:125:33: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:195:32: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[too-many-positional-arguments] src/attr/validators.py:195:32: Too many positional arguments to bound method `__init__`: expected 1, got 3
- error[too-many-positional-arguments] src/attr/validators.py:228:35: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:228:35: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:230:31: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:230:31: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:289:25: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[too-many-positional-arguments] src/attr/validators.py:289:25: Too many positional arguments to bound method `__init__`: expected 1, got 3
- error[too-many-positional-arguments] src/attr/validators.py:376:26: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[too-many-positional-arguments] src/attr/validators.py:376:26: Too many positional arguments to bound method `__init__`: expected 1, got 3
- error[too-many-positional-arguments] src/attr/validators.py:417:25: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] src/attr/validators.py:417:25: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] src/attr/validators.py:450:29: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] src/attr/validators.py:450:29: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] src/attr/validators.py:465:29: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] src/attr/validators.py:465:29: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] src/attr/validators.py:480:29: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] src/attr/validators.py:480:29: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] src/attr/validators.py:495:29: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] src/attr/validators.py:495:29: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] src/attr/validators.py:524:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:524:32: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:553:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:553:32: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:591:33: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:591:33: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] src/attr/validators.py:664:26: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] src/attr/validators.py:664:26: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] src/attr/validators.py:710:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/attr/validators.py:710:25: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] tests/test_make.py:100:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] tests/test_make.py:100:30: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] tests/test_make.py:133:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] tests/test_make.py:133:30: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] tests/test_make.py:2457:18: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] tests/test_make.py:2457:18: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] tests/test_make.py:2457:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] tests/test_make.py:2457:26: Too many positional arguments to bound method `__init__`: expected 1, got 2

graphql-core (https://github.com/graphql-python/graphql-core)
- error[too-many-positional-arguments] src/graphql/type/schema.py:458:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/graphql/type/schema.py:458:32: Too many positional arguments to bound method `__init__`: expected 1, got 2

asynq (https://github.com/quora/asynq)
- error[too-many-positional-arguments] asynq/mock_.py:128:13: Too many positional arguments to bound method `__init__`: expected 9, got 10
+ error[too-many-positional-arguments] asynq/mock_.py:128:13: Too many positional arguments to bound method `__init__`: expected 10, got 11
- error[too-many-positional-arguments] asynq/mock_.py:169:17: Too many positional arguments to bound method `__init__`: expected 9, got 10
+ error[too-many-positional-arguments] asynq/mock_.py:169:17: Too many positional arguments to bound method `__init__`: expected 10, got 11

comtypes (https://github.com/enthought/comtypes)
- error[too-many-positional-arguments] comtypes/server/automation.py:95:49: Too many positional arguments to bound method `__init__`: expected 1, got 2
+ error[too-many-positional-arguments] comtypes/server/automation.py:95:49: Too many positional arguments to bound method `__init__`: expected 2, got 3

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 645 diagnostics
+ Found 647 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[too-many-positional-arguments] src/bokeh/plotting/gmap.py:72:34: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] src/bokeh/plotting/gmap.py:72:34: Too many positional arguments to bound method `__init__`: expected 1, got 2

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[too-many-positional-arguments] ddtrace/errortracking/_handled_exceptions/bytecode_injector.py:53:42: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] ddtrace/errortracking/_handled_exceptions/bytecode_injector.py:53:42: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] ddtrace/internal/bytecode_injection/core.py:35:33: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] ddtrace/internal/bytecode_injection/core.py:35:33: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] ddtrace/internal/coverage/instrumentation_py3_11.py:202:39: Too many positional arguments to bound method `__init__`: expected 0, got 4
+ error[too-many-positional-arguments] ddtrace/internal/coverage/instrumentation_py3_11.py:202:39: Too many positional arguments to bound method `__init__`: expected 1, got 5
- error[too-many-positional-arguments] tests/internal/bytecode_injection/framework_injection/_bytecode_injection.py:35:9: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[too-many-positional-arguments] tests/internal/bytecode_injection/framework_injection/_bytecode_injection.py:35:9: Too many positional arguments to bound method `__init__`: expected 1, got 4

zulip (https://github.com/zulip/zulip)
- error[too-many-positional-arguments] zerver/tests/test_typed_endpoint.py:76:27: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[too-many-positional-arguments] zerver/tests/test_typed_endpoint.py:76:27: Too many positional arguments to bound method `__init__`: expected 1, got 3
- error[too-many-positional-arguments] zerver/tests/test_typed_endpoint.py:531:31: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] zerver/tests/test_typed_endpoint.py:531:31: Too many positional arguments to bound method `__init__`: expected 1, got 2

scipy (https://github.com/scipy/scipy)
- error[too-many-positional-arguments] scipy/stats/_multivariate.py:4511:64: Too many positional arguments to bound method `_logpdf`: expected 3, got 5
+ error[too-many-positional-arguments] scipy/stats/_multivariate.py:4511:64: Too many positional arguments to bound method `_logpdf`: expected 4, got 6

sympy (https://github.com/sympy/sympy)
- error[too-many-positional-arguments] sympy/combinatorics/coset_table.py:1140:37: Too many positional arguments to bound method `scan_and_fill`: expected 2, got 3
+ error[too-many-positional-arguments] sympy/combinatorics/coset_table.py:1140:37: Too many positional arguments to bound method `scan_and_fill`: expected 3, got 4
- error[too-many-positional-arguments] sympy/combinatorics/coset_table.py:1149:50: Too many positional arguments to bound method `scan_and_fill`: expected 2, got 3
+ error[too-many-positional-arguments] sympy/combinatorics/coset_table.py:1149:50: Too many positional arguments to bound method `scan_and_fill`: expected 3, got 4
- error[too-many-positional-arguments] sympy/integrals/manualintegrate.py:2042:70: Too many positional arguments to bound method `xreplace`: expected 1, got 2
+ error[too-many-positional-arguments] sympy/integrals/manualintegrate.py:2042:70: Too many positional arguments to bound method `xreplace`: expected 2, got 3
- error[too-many-positional-arguments] sympy/physics/quantum/matrixutils.py:74:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] sympy/physics/quantum/matrixutils.py:74:23: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] sympy/physics/quantum/matrixutils.py:79:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[too-many-positional-arguments] sympy/physics/quantum/matrixutils.py:79:23: Too many positional arguments to bound method `__init__`: expected 1, got 2
- error[too-many-positional-arguments] sympy/plotting/intervalmath/tests/test_interval_membership.py:11:62: Too many positional arguments to bound method `__init__`: expected 2, got 3
+ error[too-many-positional-arguments] sympy/plotting/intervalmath/tests/test_interval_membership.py:11:62: Too many positional arguments to bound method `__init__`: expected 3, got 4
- error[too-many-positional-arguments] sympy/polys/polyclasses.py:2445:48: Too many positional arguments to bound method `__init__`: expected 3, got 4
+ error[too-many-positional-arguments] sympy/polys/polyclasses.py:2445:48: Too many positional arguments to bound method `__init__`: expected 4, got 5

manticore (https://github.com/trailofbits/manticore)
- error[too-many-positional-arguments] manticore/native/cpu/arm.py:408:47: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[too-many-positional-arguments] manticore/native/cpu/arm.py:408:47: Too many positional arguments to bound method `__init__`: expected 1, got 3
- error[too-many-positional-arguments] manticore/native/cpu/arm.py:408:80: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[too-many-positional-arguments] manticore/native/cpu/arm.py:408:80: Too many positional arguments to bound method `__init__`: expected 1, got 3

```
</details>


---

_Merged by @carljm on 2025-05-15 02:51_

---

_Closed by @carljm on 2025-05-15 02:51_

---

_Branch deleted on 2025-05-15 07:40_

---
