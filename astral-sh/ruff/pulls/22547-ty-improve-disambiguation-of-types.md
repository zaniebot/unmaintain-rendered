```yaml
number: 22547
title: "[ty] Improve disambiguation of types"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
base: main
head: alex/more-class-qualification
created_at: 2026-01-13T12:42:22Z
updated_at: 2026-01-13T13:11:04Z
url: https://github.com/astral-sh/ruff/pull/22547
synced_at: 2026-01-13T13:22:50Z
```

# [ty] Improve disambiguation of types

---

_@AlexWaygood_

## Summary

This PR improves some details around our disambiguation of types in diagnostics.

---

Firstly, in cases where the fully qualified class name is not enough to disambiguate two types, we now provide the column number as well as the filename and line number. Most of the time, two classes aren't defined on the same line... but once https://github.com/astral-sh/ruff/pull/22537 lands, that will no longer be true. In this snippet, there are two distinct classes named `Url` which are different types, but which have exactly the same qualified name, defining file, and line number:

```py
from collections import namedtuple

class Url(namedtuple("Url", "host str")): ...
```

The only way to distinguish these two classes in diagnostics, etc., is by using the column number where the class definition starts (column 11 for the inner, inline, `Url` class definition created by the `namedtuple` call; column 1 for the outer `Url` class created by the `class` statement).

---

Secondly, we now use fully qualified names etc. to disambiguate classes in `reveal_mro` calls when necessary. This is an internal-only change, but it'll be useful for testing purposes in #22327.

## Test Plan

Mdtests updated


---

_Review requested from @carljm by @AlexWaygood on 2026-01-13 12:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-13 12:42_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-13 12:42_

---

_Label `ty` added by @AlexWaygood on 2026-01-13 12:42_

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-13 12:42_

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 12:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-13 12:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_annotations.py:644:13: error[invalid-assignment] Object of type `<class 'tests.test_annotations.TestAnnotations.<locals of function 'test_init_type_hints_fake_module'>.C @ tests/test_annotations.py:640'>` is not assignable to `<class 'tests.test_annotations.TestAnnotations.<locals of function 'test_init_type_hints_fake_module'>.C @ tests/test_annotations.py:640'>`
+ tests/test_annotations.py:644:13: error[invalid-assignment] Object of type `<class 'tests.test_annotations.TestAnnotations.<locals of function 'test_init_type_hints_fake_module'>.C @ tests/test_annotations.py:640:15'>` is not assignable to `<class 'tests.test_annotations.TestAnnotations.<locals of function 'test_init_type_hints_fake_module'>.C @ tests/test_annotations.py:640:15'>`
- tests/test_make.py:620:13: error[invalid-assignment] Object of type `<class 'tests.test_make.TestAttributes.<locals of function 'test_adds_all_by_default'>.C @ tests/test_make.py:615'>` is not assignable to `<class 'tests.test_make.TestAttributes.<locals of function 'test_adds_all_by_default'>.C @ tests/test_make.py:615'>`
+ tests/test_make.py:620:13: error[invalid-assignment] Object of type `<class 'tests.test_make.TestAttributes.<locals of function 'test_adds_all_by_default'>.C @ tests/test_make.py:615:15'>` is not assignable to `<class 'tests.test_make.TestAttributes.<locals of function 'test_adds_all_by_default'>.C @ tests/test_make.py:615:15'>`
- tests/test_make.py:675:13: error[invalid-assignment] Object of type `<class 'tests.test_make.TestAttributes.<locals of function 'test_respects_init_attrs_init'>.C @ tests/test_make.py:672'>` is not assignable to `<class 'tests.test_make.TestAttributes.<locals of function 'test_respects_init_attrs_init'>.C @ tests/test_make.py:672'>`
+ tests/test_make.py:675:13: error[invalid-assignment] Object of type `<class 'tests.test_make.TestAttributes.<locals of function 'test_respects_init_attrs_init'>.C @ tests/test_make.py:672:15'>` is not assignable to `<class 'tests.test_make.TestAttributes.<locals of function 'test_respects_init_attrs_init'>.C @ tests/test_make.py:672:15'>`
- tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858] | type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858'>`
+ tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15] | type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15'>`
- tests/test_slots.py:217:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:217:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`
- tests/test_slots.py:224:17: error[invalid-argument-type] Argument to bound method `method` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:224:17: error[invalid-argument-type] Argument to bound method `method` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`
- tests/test_slots.py:225:27: error[invalid-argument-type] Argument to bound method `classmethod` is incorrect: Expected `type[tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193]`, found `type[tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193]`
+ tests/test_slots.py:225:27: error[invalid-argument-type] Argument to bound method `classmethod` is incorrect: Expected `type[tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11]`, found `type[tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11]`
- tests/test_slots.py:230:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:230:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`
- tests/test_slots.py:232:11: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:232:11: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193:11`

pandera (https://github.com/pandera-dev/pandera)
- pandera/engines/numpy_engine.py:276:15: warning[unsupported-base] Unsupported class base with type `<class 'pandera.engines.numpy_engine.Float64 @ pandera/engines/numpy_engine.py:261'> | <class 'pandera.engines.numpy_engine.Float64 @ pandera/engines/numpy_engine.py:269'>`
+ pandera/engines/numpy_engine.py:276:15: warning[unsupported-base] Unsupported class base with type `<class 'pandera.engines.numpy_engine.Float64 @ pandera/engines/numpy_engine.py:261:11'> | <class 'pandera.engines.numpy_engine.Float64 @ pandera/engines/numpy_engine.py:269:11'>`
- pandera/engines/numpy_engine.py:325:17: warning[unsupported-base] Unsupported class base with type `<class 'pandera.engines.numpy_engine.Complex128 @ pandera/engines/numpy_engine.py:310'> | <class 'pandera.engines.numpy_engine.Complex128 @ pandera/engines/numpy_engine.py:318'>`
+ pandera/engines/numpy_engine.py:325:17: warning[unsupported-base] Unsupported class base with type `<class 'pandera.engines.numpy_engine.Complex128 @ pandera/engines/numpy_engine.py:310:11'> | <class 'pandera.engines.numpy_engine.Complex128 @ pandera/engines/numpy_engine.py:318:11'>`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5367 diagnostics
+ Found 5372 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/tornado/stack_context.py:77:24: error[invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129'>` is not a valid class
+ ddtrace/contrib/internal/tornado/stack_context.py:77:24: error[invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17:11'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129:11'>` is not a valid class
- ddtrace/contrib/internal/tornado/stack_context.py:104:24: error[invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129'>` is not a valid class
+ ddtrace/contrib/internal/tornado/stack_context.py:104:24: error[invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17:11'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129:11'>` is not a valid class
- ddtrace/contrib/internal/tornado/stack_context.py:119:17: error[invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129'>` is not a valid class
+ ddtrace/contrib/internal/tornado/stack_context.py:119:17: error[invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17:11'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129:11'>` is not a valid class

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79 | bokeh.model.model.Model @ src/bokeh/model/model.pyi:40]`
+ src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7 | bokeh.model.model.Model @ src/bokeh/model/model.pyi:40:7]`
- src/bokeh/model/model.py:510:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[bokeh.model.model.Model @ src/bokeh/model/model.py:79]`, found `Iterable[bokeh.model.model.Model @ src/bokeh/model/model.pyi:40]`
+ src/bokeh/model/model.py:510:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7]`, found `Iterable[bokeh.model.model.Model @ src/bokeh/model/model.pyi:40:7]`
- src/bokeh/model/model.py:510:21: error[invalid-argument-type] Argument to function `find` is incorrect: Expected `Iterable[bokeh.model.model.Model @ src/bokeh/model/model.pyi:40]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79]`
+ src/bokeh/model/model.py:510:21: error[invalid-argument-type] Argument to function `find` is incorrect: Expected `Iterable[bokeh.model.model.Model @ src/bokeh/model/model.pyi:40:7]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7]`

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/types/generic.py:1572:21: warning[unsupported-base] Unsupported class base with type `<class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:24'> | <class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:28'>`
+ ibis/expr/types/generic.py:1572:21: warning[unsupported-base] Unsupported class base with type `<class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:24:11'> | <class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:28:11'>`
- ibis/expr/types/relations.py:517:19: warning[unsupported-base] Unsupported class base with type `<class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:24'> | <class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:28'>`
+ ibis/expr/types/relations.py:517:19: warning[unsupported-base] Unsupported class base with type `<class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:24:11'> | <class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:28:11'>`

sympy (https://github.com/sympy/sympy)
- sympy/core/tests/test_function.py:145:31: error[unresolved-attribute] Object of type `sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:138 | sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:147 | sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:156` has no attribute `nargs`
+ sympy/core/tests/test_function.py:145:31: error[unresolved-attribute] Object of type `sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:138:11 | sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:147:11 | sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:156:11` has no attribute `nargs`
- sympy/core/tests/test_function.py:154:31: error[unresolved-attribute] Object of type `sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:147 | sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:156` has no attribute `nargs`
+ sympy/core/tests/test_function.py:154:31: error[unresolved-attribute] Object of type `sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:147:11 | sympy.core.tests.test_function.<locals of function 'test_Function'>.myfunc @ sympy/core/tests/test_function.py:156:11` has no attribute `nargs`
- sympy/parsing/c/c_parser.py:1056:15: warning[possibly-missing-attribute] Attribute `parse` may be missing on object of type `sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:85 | sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:1036`
+ sympy/parsing/c/c_parser.py:1056:15: warning[possibly-missing-attribute] Attribute `parse` may be missing on object of type `sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:85:11 | sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:1036:11`
- sympy/parsing/c/c_parser.py:1058:15: warning[possibly-missing-attribute] Attribute `parse_str` may be missing on object of type `sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:85 | sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:1036`
+ sympy/parsing/c/c_parser.py:1058:15: warning[possibly-missing-attribute] Attribute `parse_str` may be missing on object of type `sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:85:11 | sympy.parsing.c.c_parser.CCodeConverter @ sympy/parsing/c/c_parser.py:1036:11`
- sympy/parsing/fortran/fortran_parser.py:323:5: warning[possibly-missing-attribute] Attribute `visit` may be missing on object of type `sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:58 | sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:298`
+ sympy/parsing/fortran/fortran_parser.py:323:5: warning[possibly-missing-attribute] Attribute `visit` may be missing on object of type `sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:58:11 | sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:298:11`
- sympy/parsing/fortran/fortran_parser.py:324:15: warning[possibly-missing-attribute] Attribute `ret_ast` may be missing on object of type `sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:58 | sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:298`
+ sympy/parsing/fortran/fortran_parser.py:324:15: warning[possibly-missing-attribute] Attribute `ret_ast` may be missing on object of type `sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:58:11 | sympy.parsing.fortran.fortran_parser.ASR2PyVisitor @ sympy/parsing/fortran/fortran_parser.py:298:11`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-13 12:59_

---
