```yaml
number: 20476
title: "[ty] Use type context for inference of generic function calls"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/bidirectional-inference
created_at: 2025-09-18T20:57:10Z
updated_at: 2025-09-19T21:00:39Z
url: https://github.com/astral-sh/ruff/pull/20476
synced_at: 2026-01-12T15:57:02Z
```

# [ty] Use type context for inference of generic function calls

---

_@ibraheemdev_

## Summary

Part of https://github.com/astral-sh/ty/issues/168. This PR allows widening the type of a generic function call based on a type annotation, e.g.,
```py
def f[T](x: T) -> list[T]:
    return [x]

v: list[int] = f(True)
reveal_type(v)  # list[int]
```

---

_Label `ty` added by @ibraheemdev on 2025-09-18 20:57_

---

_Review requested from @carljm by @ibraheemdev on 2025-09-18 20:57_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-09-18 20:57_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-18 20:57_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-18 20:57_

---

_Comment by @github-actions[bot] on 2025-09-18 20:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-19 17:26:52.715647243 +0000
+++ new-output.txt	2025-09-19 17:26:55.828672752 +0000
@@ -240,8 +240,8 @@
 dataclasses_transform_class.py:119:18: error[unknown-argument] Argument `id` does not match any known parameter of bound method `__init__`
 dataclasses_transform_class.py:119:24: error[unknown-argument] Argument `name` does not match any known parameter of bound method `__init__`
 dataclasses_transform_converter.py:25:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@model_field`
-dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter1() -> int`
-dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter2(*, x: int) -> int`
+dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> int`, found `def bad_converter1() -> int`
+dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> int`, found `def bad_converter2(*, x: int) -> int`
 dataclasses_transform_converter.py:107:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
 dataclasses_transform_converter.py:108:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
 dataclasses_transform_converter.py:109:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
@@ -251,7 +251,7 @@
 dataclasses_transform_converter.py:116:1: error[invalid-assignment] Object of type `Literal[b"f6"]` is not assignable to attribute `field3` of type `ConverterClass`
 dataclasses_transform_converter.py:119:1: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `field3` of type `ConverterClass`
 dataclasses_transform_converter.py:121:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 7
-dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
+dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> int`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo(unsupported type[X] special form)`
 dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 dataclasses_transform_func.py:53:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
@@ -270,7 +270,7 @@
 dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
 dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
 dataclasses_usage.py:83:13: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_usage.py:88:5: error[invalid-assignment] Object of type `dataclasses.Field[Literal[""]]` is not assignable to `int`
+dataclasses_usage.py:88:14: error[no-matching-overload] No overload of function `field` matches arguments
 dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_usage.py:130:1: error[missing-argument] No argument provided for required parameter `y` of bound method `__init__`
 dataclasses_usage.py:179:6: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
@@ -619,7 +619,7 @@
 literals_literalstring.py:120:22: error[invalid-argument-type] Argument to function `literal_identity` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `TLiteral`
 literals_literalstring.py:130:5: error[invalid-assignment] Object of type `Container[str]` is not assignable to `Container[LiteralString]`
 literals_literalstring.py:134:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `T`
-literals_literalstring.py:138:1: error[invalid-assignment] Object of type `list[str]` is not assignable to `list[LiteralString]`
+literals_literalstring.py:138:1: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[LiteralString]`
 literals_literalstring.py:167:1: error[type-assertion-failure] Argument does not have asserted type `A`
 literals_literalstring.py:171:5: error[invalid-assignment] Object of type `list[LiteralString]` is not assignable to `list[str]`
 literals_parameterizations.py:41:15: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
```
</details>


---

_@ibraheemdev reviewed on 2025-09-18 21:06_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:267 on 2025-09-18 21:06_

Ideally we would infer the RHS without the type context if it is not assignable to the type annotation, like we do for collection literals. However, this is a little difficult as we also want the type annotation to be the first type that we use for inference, so that the inferred type more closely matches the type annotation.

I think the only way this can work is if we perform inference twice, first without the type context, and then again with it only if the unannotated inferred type is assignable to the type annotation. However, I'm not sure that's worth the extra work for this specific error message.

Maybe we could instead generalize this to performing type inference on the RHS directly when emitting the diagnostic?

---

_Comment by @github-actions[bot] on 2025-09-18 21:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/dataclass_transform_example.py:21:13: info[revealed-type] Revealed type: `(self: DefineConverter, with_converter: int = Unknown) -> None`
+ tests/dataclass_transform_example.py:21:13: info[revealed-type] Revealed type: `(self: DefineConverter, with_converter: int = int) -> None`
+ tests/test_next_gen.py:379:14: error[unresolved-attribute] Type `int` has no attribute `validator`
- Found 564 diagnostics
+ Found 565 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:2530:13: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `(...) -> Future[T_Retval@run_async_from_thread]`, found `def run_coroutine_threadsafe[_T](coro: Coroutine[Any, Any, _T@run_coroutine_threadsafe], loop: AbstractEventLoop) -> Future[_T@run_coroutine_threadsafe]`
+ src/anyio/from_thread.py:448:19: error[no-matching-overload] No overload of function `field` matches arguments
- Found 225 diagnostics
+ Found 227 diagnostics

kornia (https://github.com/kornia/kornia)
+ kornia/config.py:68:36: error[no-matching-overload] No overload of function `field` matches arguments
- Found 783 diagnostics
+ Found 784 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/request.py:25:33: error[no-matching-overload] No overload of function `field` matches arguments
+ boostedblob/request.py:29:34: error[no-matching-overload] No overload of function `field` matches arguments
+ boostedblob/request.py:76:33: error[no-matching-overload] No overload of function `field` matches arguments
+ boostedblob/request.py:80:34: error[no-matching-overload] No overload of function `field` matches arguments
+ boostedblob/request.py:85:51: error[no-matching-overload] No overload of function `field` matches arguments
- Found 71 diagnostics
+ Found 76 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:594:9: error[invalid-assignment] Object of type `list[dict[@Todo(dict literal key type), @Todo(dict literal value type)]]` is not assignable to `list[DockerParameter]`
+ paasta_tools/utils.py:594:9: error[invalid-assignment] Object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]` is not assignable to `list[DockerParameter]`

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/rich/progress.py:980:20: error[no-matching-overload] No overload of function `field` matches arguments
- Found 434 diagnostics
+ Found 435 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/longlinemarker.py:56:17: error[invalid-assignment] Object of type `list[str]` is not assignable to `list[Literal["bgcolor", "color", "border"]]`
+ porcupine/plugins/longlinemarker.py:56:17: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[Literal["bgcolor", "color", "border"]]`

rich (https://github.com/Textualize/rich)
+ rich/progress.py:980:20: error[no-matching-overload] No overload of function `field` matches arguments
- Found 309 diagnostics
+ Found 310 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/python.py:1064:39: error[no-matching-overload] No overload of function `field` matches arguments
- Found 478 diagnostics
+ Found 479 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_generate_schema.py:126:1: error[invalid-assignment] Object of type `list[typing.Tuple | <class 'tuple'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:126:1: error[invalid-assignment] Object of type `list[Unknown | typing.Tuple | <class 'tuple'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:127:1: error[invalid-assignment] Object of type `list[typing.List | <class 'list'> | <class 'MutableSequence'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:127:1: error[invalid-assignment] Object of type `list[Unknown | typing.List | <class 'list'> | <class 'MutableSequence'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:128:1: error[invalid-assignment] Object of type `list[typing.Set | <class 'set'> | <class 'MutableSet'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:128:1: error[invalid-assignment] Object of type `list[Unknown | typing.Set | <class 'set'> | <class 'MutableSet'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:129:1: error[invalid-assignment] Object of type `list[typing.FrozenSet | <class 'frozenset'> | <class 'AbstractSet'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:129:1: error[invalid-assignment] Object of type `list[Unknown | typing.FrozenSet | <class 'frozenset'> | <class 'AbstractSet'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:130:1: error[invalid-assignment] Object of type `list[typing.Dict | <class 'dict'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:130:1: error[invalid-assignment] Object of type `list[Unknown | typing.Dict | <class 'dict'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:134:1: error[invalid-assignment] Object of type `list[typing.Type | <class 'type'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:134:1: error[invalid-assignment] Object of type `list[Unknown | typing.Type | <class 'type'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:155:1: error[invalid-assignment] Object of type `list[<class 'deque'> | typing.Deque]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:155:1: error[invalid-assignment] Object of type `list[Unknown | <class 'deque'> | typing.Deque]` is not assignable to `list[type]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/python/__init__.py:672:42: error[index-out-of-bounds] Index 0 is out of bounds for string `Literal[""]` with length 0
- Found 518 diagnostics
+ Found 517 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/auths.py:127:37: error[no-matching-overload] No overload of function `field` matches arguments
- Found 270 diagnostics
+ Found 271 diagnostics

Expression (https://github.com/cognitedata/Expression)
- tests/test_array.py:137:5: error[invalid-assignment] Object of type `TypedArray[Literal[42]]` is not assignable to `TypedArray[int]`
- tests/test_array.py:307:38: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
- tests/test_block.py:274:38: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
- tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0], Any]`
- tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error"]]`
- tests/test_try.py:13:5: error[invalid-assignment] Object of type `Try[Literal[10]]` is not assignable to `Try[int]`
- tests/test_try.py:34:5: error[invalid-assignment] Object of type `Try[Literal[10]]` is not assignable to `Try[int]`
- Found 230 diagnostics
+ Found 223 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_dtypes.py:170:1: error[invalid-assignment] Object of type `list[tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | int]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | int | None]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | float]] | @Todo(starred expression) | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | complex]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | bool]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | bool | None]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | str]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], Unknown] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], Series[Any]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`
+ tests/pandas/test_dtypes.py:170:1: error[invalid-assignment] Object of type `list[Unknown | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | int]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | int | None]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | float]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | complex]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | bool]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | bool | None]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | str]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], Unknown] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], Series[Any]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_entry_queue.py:45:43: error[invalid-argument-type] Argument to function `Factory` is incorrect: Expected `() -> RLock`, found `<class 'RLock'>`
- Found 673 diagnostics
+ Found 674 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/types/base.py:272:63: error[no-matching-overload] No overload of function `field` matches arguments
- Found 377 diagnostics
+ Found 378 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
+ src/scikit_build_core/settings/skbuild_docs_sphinx.py:27:36: error[no-matching-overload] No overload of function `field` matches arguments
+ src/scikit_build_core/settings/skbuild_docs_sphinx.py:43:30: error[no-matching-overload] No overload of function `field` matches arguments
- Found 42 diagnostics
+ Found 45 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/command/bdist_egg.py:328:1: error[invalid-assignment] Object of type `dict[LiteralString, Any | None]` is not assignable to `dict[str, None]`
- Found 775 diagnostics
+ Found 774 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/modules/_qt.py:941:9: error[invalid-assignment] Object of type `list[str | bool]` is not assignable to `list[str | Literal[False]]`
+ mesonbuild/modules/_qt.py:941:9: error[invalid-assignment] Object of type `list[Unknown | bool]` is not assignable to `list[str | Literal[False]]`

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-shell/prefect_shell/commands.py:265:9: error[invalid-argument-type] Argument to function `PrivateAttr` is incorrect: Expected `() -> AsyncExitStack[bool | None]`, found `<class 'AsyncExitStack'>`
+ src/prefect/tasks.py:1673:13: error[invalid-argument-type] Argument to function `emit_task_run_state_change_event` is incorrect: Expected `State[Any]`, found `State[Any] | None`
- Found 3037 diagnostics
+ Found 3039 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/rate_limiter.py:203:29: error[no-matching-overload] No overload of function `field` matches arguments
- Found 6664 diagnostics
+ Found 6665 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/pallas/mosaic/interpret.py:403:26: error[no-matching-overload] No overload of function `field` matches arguments
+ jax/_src/pallas/mosaic/interpret.py:419:26: error[no-matching-overload] No overload of function `field` matches arguments
+ jax/_src/pallas/mosaic/interpret.py:582:26: error[no-matching-overload] No overload of function `field` matches arguments
- jax/_src/traceback_util.py:30:1: error[invalid-assignment] Object of type `list[str | None]` is not assignable to `list[str]`
+ jax/_src/traceback_util.py:30:1: error[invalid-assignment] Object of type `list[Unknown | str | None]` is not assignable to `list[str]`
- Found 2243 diagnostics
+ Found 2246 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/frame.py:13971:5: error[invalid-assignment] Object of type `list[str]` is not assignable to `list[Literal["index", "columns"]]`
+ pandas/core/frame.py:13971:5: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[Literal["index", "columns"]]`

core (https://github.com/home-assistant/core)
+ homeassistant/helpers/service_info/ssdp.py:39:39: error[no-matching-overload] No overload of function `field` matches arguments
- Found 13448 diagnostics
+ Found 13449 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
+ Lib/_pyrepl/_threading_handler.py:35:32: error[no-matching-overload] No overload of function `field` matches arguments
- Found 23904 diagnostics
+ Found 23905 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
+ Lib/_pyrepl/_threading_handler.py:35:32: error[no-matching-overload] No overload of function `field` matches arguments
- Found 23904 diagnostics
+ Found 23905 diagnostics

CPython (peg_generator) (https://github.com/python/cpython)
+ Lib/_pyrepl/_threading_handler.py:35:32: error[no-matching-overload] No overload of function `field` matches arguments
- Found 23903 diagnostics
+ Found 23904 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@ibraheemdev reviewed on 2025-09-18 21:23_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:279 on 2025-09-18 21:23_

This works because we ignore specialization errors on the return type. pyright also infers `bool` here, but mypy interestingly still infers `int | str`.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:267 on 2025-09-18 21:25_

Yeah, I think what would make sense here is to re-infer the RHS without context if we are emitting a diagnostic. Some extra cost is fine, if it's only paid when actually emitting a diagnostic.

---

_@carljm reviewed on 2025-09-18 21:25_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:124 on 2025-09-18 21:41_

Not sure if I'm understanding this comment right, but the wording doesn't quite make sense to me. By "return type annotation" I would generally mean the `-> T` in `def f[T](x: T) -> T:`. But `return_tcx` here is not that, it's the annotated type context of the entire call expression, which would be used to help infer the type of `T`.

If I'm getting this right, I think I would name the argument `call_expression_tcx` not `return_tcx` (though I understand the rationale for the latter name, since it's the return type which will become the type of the entire call expression), and would reword this comment to something like
```suggestion
    /// The type context of the call expression is also used to infer the specialization of generic calls.
```

---

_@carljm approved on 2025-09-18 21:43_

Nice! Have you done any analysis of the ecosystem results?

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-18 22:06_

---

_Comment by @github-actions[bot] on 2025-09-18 22:12_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `no-matching-overload` | 19 | 0 | 0 |
| `invalid-assignment` | 0 | 4 | 13 |
| `invalid-argument-type` | 4 | 4 | 0 |
| `index-out-of-bounds` | 0 | 1 | 0 |
| `unresolved-attribute` | 1 | 0 | 0 |
| **Total** | **24** | **9** | **13** |

**[Full report with detailed diff](https://ibraheem-bidirectional-infer.ecosystem-663.pages.dev/diff)**


---

_Comment by @ibraheemdev on 2025-09-19 00:50_

There seems to be a common ecosystem error where we fail to type-check calls to `dataclasses.field`. This can be minimized to:
```py
class X: ...

def field[T](default_factory: Callable[[], T]) -> T:
    return default_factory()

# error[invalid-argument-type] Argument to function `field` is incorrect: Expected `() -> X`, found `<class 'X'>`
x: X = field(X)
```

However, without the type annotation, we currently infer `Unknown` for `x`, so I'm not sure if the issue is related to this PR?

---

_Comment by @carljm on 2025-09-19 16:10_

The fact that we can't infer this typevar is https://github.com/astral-sh/ty/issues/500, which we closed as duplicate of the new constraint solver.

It's a bit strange to me that we display the callable as `() -> X` and think the class `X` is not assignable to that; I'm not clear what's going on there. I do think it's probably not directly related to this PR, but clearly this PR is introducing the false positives in this case, and I think these are probably important false positives to understand and fix. Maybe the new constraint solver will just take care of this?

---

_@ibraheemdev reviewed on 2025-09-19 17:02_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:226 on 2025-09-19 17:02_

Don't love the name here.

---

_Comment by @ibraheemdev on 2025-09-19 17:24_

> It's a bit strange to me that we display the callable as `() -> X` and think the `class X` is not assignable to that; I'm not clear what's going on there. I do think it's probably not directly related to this PR, but clearly this PR is introducing the false positives in this case, and I think these are probably important false positives to understand and fix. Maybe the new constraint solver will just take care of this?

It looks like we correctly infer the type variable now due to the type annotation, but the issue remains without any generics, so I think this is unrelated:
```py
class X:
    ...

def f(callable: Callable[[], X]) -> X:
    return callable()

# error[invalid-argument-type]: Argument to function `f` is incorrect
x = f(X)
```
It seems to work if I add an explicit `__new__` or `__init__` method to `X`, I'm not sure if there's an open issue about this.

Edit: Opened https://github.com/astral-sh/ty/issues/1210.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-19 19:17_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-19 19:17_

---

_Comment by @ibraheemdev on 2025-09-19 20:39_

There's another issue here with:
```py
def f[T](callable: Callable[[], T]) -> T:
    return callable()

# Expected `() -> Mapping[str, str]`, found `<class 'dict'>`
x: Mapping[str, str] = f(dict)
```

After this PR, we infer `T` to be `Mapping[str, str]` based on the type annotation, and so expect a `Callable[[], Mapping[str, str]]`. This one should be fixed by https://github.com/astral-sh/ty/issues/500, because we should also account for the return type of the provided callable (i.e. `dict`).

Given that both of those issues are unrelated to the changes here, I think this PR should be good to merge.

---

_Merged by @ibraheemdev on 2025-09-19 21:00_

---

_Closed by @ibraheemdev on 2025-09-19 21:00_

---

_Branch deleted on 2025-09-19 21:00_

---
