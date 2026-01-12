```yaml
number: 18600
title: "[ty] Homogeneous and mixed tuples"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/tuple-spec
created_at: 2025-06-09T21:08:40Z
updated_at: 2025-06-23T13:27:28Z
url: https://github.com/astral-sh/ruff/pull/18600
synced_at: 2026-01-12T15:56:22Z
```

# [ty] Homogeneous and mixed tuples

---

_@dcreager_

We already had support for homogeneous tuples (`tuple[int, ...]`). This PR extends this to also support mixed tuples (`tuple[str, str, *tuple[int, ...], str str]`).

A mixed tuple consists of a fixed-length (possibly empty) prefix and suffix, and a variable-length portion in the middle. Every element of the variable-length portion must be of the same type. A homogeneous tuple is then just a mixed tuple with an empty prefix and suffix.

The new data representation uses different Rust types for a fixed-length (aka heterogeneous) tuple. Another option would have been to use the `VariableLengthTuple` representation for all tuples, and to wrap the "variable + suffix" portion in an `Option`. I don't think that would simplify the method implementations much, though, since we would still have a 2×2 case analysis for most of them.

One wrinkle is that the definition of the `tuple` class in the typeshed has a single typevar, and canonically represents a homogeneous tuple. When getting the class of a tuple instance, that means that we have to summarize our detailed mixed tuple type information into its "homogeneous supertype". (We were already doing this for heterogeneous types.)

A similar thing happens when concatenating two mixed tuples: the variable-length portion and suffix of the LHS, and the prefix and variable-length portion of the RHS, all get unioned into the variable-length portion of the result. The LHS prefix and RHS suffix carry through unchanged.

---

_Comment by @github-actions[bot] on 2025-06-09 21:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- error[invalid-argument-type] parso/python/diff.py:884:35: Argument to bound method `__init__` is incorrect: Expected `tuple[int, int]`, found `tuple[Unknown, ...]`
- Found 80 diagnostics
+ Found 79 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[call-non-callable] src/anyio/_backends/_asyncio.py:2223:49: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/anyio/_backends/_asyncio.py:2223:49: Object of type `GenericAlias` is not callable
- Found 103 diagnostics
+ Found 101 diagnostics

beartype (https://github.com/beartype/beartype)
+ error[invalid-type-form] beartype/_data/hint/datahintpep.py:250:20: Variable of type `_SpecialForm` is not allowed in a type expression
- Found 580 diagnostics
+ Found 581 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[unsupported-operator] more_itertools/more.pyi:164:18: Operator `|` is unsupported between objects of type `<class 'type'>` and `<class 'tuple[@Todo(Generic tuple specializations), ...]'>`
+ error[unsupported-operator] more_itertools/more.pyi:164:18: Operator `|` is unsupported between objects of type `<class 'type'>` and `<class 'tuple[@Todo(Support for `typing.TypeAlias`), ...]'>`

Expression (https://github.com/cognitedata/Expression)
- error[invalid-argument-type] expression/extra/option/pipeline.py:91:19: Argument to function `reduce` is incorrect: Expected `(def Some(value: _T1) -> Option[_T1], (Any, /) -> Option[Any], /) -> def Some(value: _T1) -> Option[_T1]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
+ error[invalid-argument-type] expression/extra/option/pipeline.py:91:19: Argument to function `reduce` is incorrect: Expected `(def Some(value: _T1) -> Option[_T1], Unknown, /) -> def Some(value: _T1) -> Option[_T1]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- error[invalid-argument-type] expression/extra/result/pipeline.py:96:19: Argument to function `reduce` is incorrect: Expected `(def Ok(value: _TSource) -> Result[_TSource, Any], (Any, /) -> Result[Any, Any], /) -> def Ok(value: _TSource) -> Result[_TSource, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
+ error[invalid-argument-type] expression/extra/result/pipeline.py:96:19: Argument to function `reduce` is incorrect: Expected `(def Ok(value: _TSource) -> Result[_TSource, Any], Unknown, /) -> def Ok(value: _TSource) -> Result[_TSource, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`

websockets (https://github.com/aaugustin/websockets)
- warning[redundant-cast] src/websockets/legacy/auth.py:163:37: Value is already of type `Iterable[Unknown | tuple[@Todo(Generic tuple specializations), ...]]`
+ warning[redundant-cast] src/websockets/legacy/auth.py:163:37: Value is already of type `Iterable[Unknown | tuple[str, str]]`

black (https://github.com/psf/black)
+ error[unsupported-operator] src/black/parsing.py:80:37: Operator `-` is unsupported between objects of type `Unknown | str | int` and `Literal[1]`
+ error[invalid-argument-type] src/blib2to3/pgen2/parse.py:34:17: Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | str | None | tuple[str, tuple[int, int]] | list[@Todo(Inference of subscript on special form)]`
+ error[invalid-argument-type] src/blib2to3/pgen2/parse.py:34:31: Argument to bound method `__init__` is incorrect: Expected `list[@Todo(Inference of subscript on special form)]`, found `(Unknown & ~None) | int | str | tuple[str, tuple[int, int]] | list[@Todo(Inference of subscript on special form)]`
- Found 68 diagnostics
+ Found 71 diagnostics

starlette (https://github.com/encode/starlette)
+ warning[possibly-unbound-attribute] starlette/middleware/base.py:134:27: Attribute `send` on type `Unknown | MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] starlette/middleware/base.py:151:33: Attribute `receive` on type `Unknown | MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] starlette/middleware/base.py:154:37: Attribute `receive` on type `Unknown | MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] starlette/testclient.py:138:40: Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] starlette/testclient.py:138:60: Attribute `send` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ error[invalid-assignment] tests/test_websockets.py:213:5: Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is not assignable to `ObjectSendStream[MutableMapping[str, Any]]`
+ error[invalid-assignment] tests/test_websockets.py:213:18: Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is not assignable to `ObjectReceiveStream[MutableMapping[str, Any]]`
- Found 166 diagnostics
+ Found 173 diagnostics

kornia (https://github.com/kornia/kornia)
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[unsupported-operator] kornia/augmentation/utils/param_validation.py:139:16: Operator `<` is not supported for types `tuple[Any, ...]` and `int`, in comparing `Unknown | int | float | tuple[Any, ...]` with `Literal[0]`
+ error[unsupported-operator] kornia/augmentation/utils/param_validation.py:139:16: Operator `<` is not supported for types `tuple[Any, ...]` and `Literal[0]`, in comparing `Unknown | int | float | tuple[Any, ...]` with `Literal[0]`
- error[invalid-return-type] kornia/feature/dedode/transformer/dinov2.py:309:20: Return type does not match returned value: expected `tuple[Unknown | tuple[Unknown]]`, found `tuple[Unknown, ...]`
- error[invalid-return-type] kornia/feature/dedode/transformer/dinov2.py:310:16: Return type does not match returned value: expected `tuple[Unknown | tuple[Unknown]]`, found `tuple[Unknown, ...]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
- Found 948 diagnostics
+ Found 936 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- error[invalid-argument-type] dedupe/convenience.py:77:16: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[Unknown]`
- error[invalid-argument-type] dedupe/convenience.py:77:19: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[Unknown]`
- Found 64 diagnostics
+ Found 62 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ error[unsupported-operator] comtypes/tools/codegenerator/comments.py:24:71: Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["out"]` with `@Todo(Type::Intersection.call()) | str | list[str] | None`
+ error[unsupported-operator] comtypes/tools/codegenerator/comments.py:25:72: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["out"]` with `@Todo(Type::Intersection.call()) | str | list[str] | None`
- Found 545 diagnostics
+ Found 547 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-assignment] strawberry/exceptions/handler.py:90:9: Object of type `def strawberry_threading_exception_handler(args: tuple[type[BaseException], BaseException | None, TracebackType | None, Thread | None]) -> None` is not assignable to attribute `excepthook` of type `(_ExceptHookArgs, /) -> Any`
+ error[unresolved-attribute] strawberry/tools/merge_types.py:26:11: Type `type` has no attribute `__strawberry_definition__`

sockeye (https://github.com/awslabs/sockeye)
- warning[division-by-zero] sockeye/data_io.py:531:25: Cannot divide object of type `Literal[0]` by zero
- warning[division-by-zero] sockeye/data_io.py:532:25: Cannot divide object of type `Literal[0]` by zero
- Found 365 diagnostics
+ Found 363 diagnostics

trio (https://github.com/python-trio/trio)
+ error[invalid-argument-type] src/trio/_channel.py:546:38: Argument to bound method `__init__` is incorrect: Expected `MemoryReceiveChannel[Unknown]`, found `MemorySendChannel[Unknown] | MemoryReceiveChannel[Unknown]`
- error[call-non-callable] src/trio/_channel.py:534:32: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_channel.py:534:32: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_channel.py:534:32: Object of type `GenericAlias` is not callable
+ error[invalid-argument-type] src/trio/_core/_tests/test_guest_mode.py:524:66: Argument to function `aio_pingpong` is incorrect: Expected `MemorySendChannel[int]`, found `MemorySendChannel[int] | MemoryReceiveChannel[int]`
- error[call-non-callable] src/trio/_core/_tests/test_guest_mode.py:521:29: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_core/_tests/test_guest_mode.py:521:29: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_core/_tests/test_guest_mode.py:521:29: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_dtls.py:669:26: Object of type `GenericAlias` is not callable
+ warning[possibly-unbound-attribute] src/trio/_dtls.py:748:9: Attribute `send_nowait` on type `Unknown | MemorySendChannel[DTLSChannel] | MemoryReceiveChannel[DTLSChannel]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_dtls.py:789:29: Attribute `send_nowait` on type `Unknown | MemorySendChannel[bytes] | MemoryReceiveChannel[bytes]` is possibly unbound
- error[call-non-callable] src/trio/_tests/test_channel.py:26:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:26:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:26:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:64:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:64:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:64:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:78:37: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:78:37: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:78:37: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:102:41: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:102:41: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:102:41: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:124:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:124:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:124:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:143:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:143:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:143:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:160:15: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:160:15: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:160:15: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:182:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:182:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:182:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:201:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:201:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:201:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:218:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:218:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:218:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:232:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:232:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:232:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:261:13: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:261:13: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:261:13: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:282:13: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:282:13: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:282:13: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:301:21: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:301:21: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:301:21: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:313:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:313:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:313:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:365:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:365:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:365:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:391:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:391:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:391:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:405:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:405:12: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_channel.py:405:12: Object of type `GenericAlias` is not callable
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:30:5: Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:32:15: Attribute `send` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:34:9: Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:37:22: Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:38:12: Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:40:9: Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:42:5: Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:45:15: Attribute `send` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:47:9: Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:52:12: Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:54:15: Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:57:15: Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:59:9: Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:66:15: Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:68:11: Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:108:23: Attribute `send` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:132:9: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:134:15: Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:138:9: Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:140:15: Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:151:9: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:153:15: Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:168:9: Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:170:15: Attribute `receive` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:190:9: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:192:15: Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:196:9: Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:198:15: Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:209:9: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:211:15: Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:226:9: Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:228:15: Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:237:5: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:249:5: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:255:9: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:266:19: Attribute `send` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:269:15: Attribute `send` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:276:22: Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:287:19: Attribute `receive` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:290:22: Attribute `receive` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:297:15: Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:306:13: Attribute `send_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:324:5: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:338:9: Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:340:28: Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:341:28: Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:350:13: Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:355:28: Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:366:5: Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:367:12: Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:368:5: Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:369:12: Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:383:9: Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:385:13: Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:392:5: Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:394:9: Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:396:28: Attribute `send` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:398:16: Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:400:13: Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:401:23: Attribute `receive` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:407:9: Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:409:9: Attribute `send_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:418:26: Attribute `receive` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_tests/test_channel.py:420:9: Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- error[call-non-callable] src/trio/_tests/test_highlevel_serve_listeners.py:41:31: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_sync.py:423:26: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_sync.py:439:26: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/test_sync.py:454:26: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/type_tests/open_memory_channel.py:4:8: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/type_tests/open_memory_channel.py:4:8: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/_tests/type_tests/open_memory_channel.py:4:8: Object of type `GenericAlias` is not callable
- error[call-non-callable] src/trio/testing/_fake_net.py:230:54: Object of type `GenericAlias` is not callable
- Found 1010 diagnostics
+ Found 1009 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-assignment] pydantic/_internal/_generics.py:97:1: Object of type `ContextVar[None]` is not assignable to `ContextVar[WeakValueDictionary[tuple[@Todo(Generic tuple specializations), ...], type[BaseModel]] | None]`
+ error[invalid-assignment] pydantic/_internal/_generics.py:97:1: Object of type `ContextVar[None]` is not assignable to `ContextVar[WeakValueDictionary[tuple[Any, Any, tuple[Any, ...]], type[BaseModel]] | None]`
- error[invalid-return-type] pydantic/version.py:84:12: Return type does not match returned value: expected `tuple[int, int, int]`, found `tuple[Unknown, ...]`
- Found 731 diagnostics
+ Found 730 diagnostics

rich (https://github.com/Textualize/rich)
+ error[invalid-argument-type] tests/test_syntax.py:169:38: Argument to bound method `get_style_for_token` is incorrect: Expected `tuple[str, ...]`, found `Literal["abc"]`
- Found 373 diagnostics
+ Found 374 diagnostics

flake8 (https://github.com/pycqa/flake8)
- error[invalid-return-type] src/flake8/checker.py:605:16: Return type does not match returned value: expected `tuple[int, int]`, found `int & tuple[Unknown, ...]`
+ error[unsupported-operator] src/flake8/checker.py:609:12: Operator `<=` is not supported for types `int` and `tuple[int, int]`, in comparing `int & ~tuple[Unknown, ...]` with `Unknown | int | tuple[int, int]`
+ warning[possibly-unbound-implicit-call] src/flake8/checker.py:615:13: Method `__getitem__` of type `Unknown | int | tuple[int, int]` is possibly unbound
+ error[unsupported-operator] src/flake8/checker.py:615:26: Operator `-` is unsupported between objects of type `Unknown | int` and `Unknown | int | tuple[int, int]`
+ warning[possibly-unbound-implicit-call] src/flake8/checker.py:615:26: Method `__getitem__` of type `Unknown | int | tuple[int, int]` is possibly unbound
+ error[not-iterable] src/flake8/processor.py:166:34: Object of type `int` is not iterable
- Found 43 diagnostics
+ Found 47 diagnostics

yarl (https://github.com/aio-libs/yarl)
- error[invalid-argument-type] tests/test_pickle.py:29:20: Argument to bound method `__setstate__` is incorrect: Expected `tuple[Unknown | tuple[@Todo(Generic tuple specializations), ...]] | tuple[None, _InternalURLCache]`, found `tuple[None, dict[Unknown, Unknown]]`
+ error[invalid-argument-type] tests/test_pickle.py:29:20: Argument to bound method `__setstate__` is incorrect: Expected `tuple[Unknown | tuple[str, str, str, str, str]] | tuple[None, _InternalURLCache]`, found `tuple[None, dict[Unknown, Unknown]]`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- warning[possibly-unbound-attribute] src/hydra_zen/structured_configs/_implementations.py:2146:28: Attribute `get` on type `DataclassOptions | None` is possibly unbound
- warning[possibly-unbound-attribute] src/hydra_zen/structured_configs/_implementations.py:3153:21: Attribute `get` on type `DataclassOptions | None` is possibly unbound
- Found 597 diagnostics
+ Found 595 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[invalid-return-type] tests/conftest.py:432:12: Return type does not match returned value: expected `tuple[int, int, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
+ error[invalid-return-type] tests/conftest.py:432:12: Return type does not match returned value: expected `tuple[int, int, int]`, found `Unknown | tuple[int | str, ...]`

urllib3 (https://github.com/urllib3/urllib3)
+ warning[possibly-unbound-attribute] src/urllib3/filepost.py:72:9: Attribute `write` on type `Unknown | tuple[bytes, int]` is possibly unbound
+ error[invalid-argument-type] src/urllib3/filepost.py:72:16: Argument to bound method `__call__` is incorrect: Expected `str`, found `BytesIO`
+ warning[possibly-unbound-attribute] src/urllib3/filepost.py:79:13: Attribute `write` on type `Unknown | tuple[bytes, int]` is possibly unbound
+ error[invalid-argument-type] src/urllib3/filepost.py:79:20: Argument to bound method `__call__` is incorrect: Expected `str`, found `BytesIO`
- Found 506 diagnostics
+ Found 510 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-return-type] references/depth/stereo/transforms.py:108:16: Return type does not match returned value: expected `tuple[@Todo(Inference of subscript on special form), tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[@Todo(Inference of subscript on special form), tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[Unknown, ...]]`
- error[invalid-return-type] references/depth/stereo/transforms.py:124:16: Return type does not match returned value: expected `tuple[@Todo(Inference of subscript on special form), tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown, ...], tuple[Unknown, ...]]`
- error[invalid-return-type] references/depth/stereo/transforms.py:190:16: Return type does not match returned value: expected `tuple[@Todo(Inference of subscript on special form), tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[()] | tuple[Unknown] | tuple[None], tuple[()] | tuple[Unknown] | tuple[None]]`
+ error[invalid-return-type] references/depth/stereo/transforms.py:190:16: Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[()] | tuple[Unknown] | tuple[None], tuple[()] | tuple[Unknown] | tuple[None]]`
- error[invalid-return-type] references/depth/stereo/transforms.py:485:16: Return type does not match returned value: expected `tuple[@Todo(Inference of subscript on special form), tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[()] | tuple[Unknown], tuple[()] | tuple[Unknown] | tuple[None], tuple[()] | tuple[Unknown] | tuple[None]]`
+ error[invalid-return-type] references/depth/stereo/transforms.py:485:16: Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[()] | tuple[Unknown], tuple[()] | tuple[Unknown] | tuple[None], tuple[()] | tuple[Unknown] | tuple[None]]`
- error[invalid-return-type] references/depth/stereo/transforms.py:601:16: Return type does not match returned value: expected `tuple[@Todo(Inference of subscript on special form), tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[(@Todo(Inference of subscript on special form) & None) | Unknown, (@Todo(Inference of subscript on special form) & None) | Unknown], tuple[()] | tuple[(@Todo(Inference of subscript on special form) & None) | Unknown]]`
+ error[invalid-return-type] references/depth/stereo/transforms.py:601:16: Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[(@Todo(Inference of subscript on special form) & None) | Unknown, (@Todo(Inference of subscript on special form) & None) | Unknown], tuple[()] | tuple[(@Todo(Inference of subscript on special form) & None) | Unknown]]`
- error[invalid-assignment] test/datasets_utils.py:1043:9: Object of type `LiteralString` is not assignable to `tuple[str, ...]`
- Found 1541 diagnostics
+ Found 1538 diagnostics

paasta (https://github.com/yelp/paasta)
- error[unresolved-reference] paasta_tools/paastaapi/model/instance_status_adhoc.py:152:30: Name `_path_to_item` used when not defined
- error[unresolved-reference] paasta_tools/paastaapi/model/instance_tasks.py:147:30: Name `_path_to_item` used when not defined
- error[unresolved-reference] paasta_tools/paastaapi/model/resource.py:152:30: Name `_path_to_item` used when not defined
+ error[invalid-return-type] paasta_tools/utils.py:3111:12: Return type does not match returned value: expected `str`, found `str | int`
- Found 954 diagnostics
+ Found 952 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[invalid-argument-type] src/schemathesis/specs/openapi/patterns.py:325:42: Argument to function `_handle_repeat_quantifier` is incorrect: Expected `tuple[int, int, tuple[Unknown, ...]]`, found `tuple[Unknown, ...]`
- Found 315 diagnostics
+ Found 314 diagnostics

jinja (https://github.com/pallets/jinja)
- warning[possibly-unresolved-reference] src/jinja2/filters.py:1740:17: Name `name` used when possibly not defined
- Found 209 diagnostics
+ Found 208 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ error[invalid-assignment] pwndbg/commands/hijack_fd.py:222:13: Object of type `str | int | bytes` is not assignable to `str`
- error[invalid-assignment] pwndbg/emu/emulator.py:514:9: Object of type `tuple[Unknown, ...]` is not assignable to `tuple[str]`
- error[invalid-return-type] pwndbg/emu/emulator.py:841:20: Return type does not match returned value: expected `tuple[int, int]`, found `InstructionExecutedResult`
- error[invalid-return-type] pwndbg/emu/emulator.py:869:24: Return type does not match returned value: expected `tuple[int, int]`, found `InstructionExecutedResult`
+ error[unsupported-operator] pwndbg/integration/binja.py:451:17: Operator `-` is unsupported between objects of type `Unknown | int | None` and `int`
+ error[unsupported-operator] pwndbg/integration/binja.py:452:15: Operator `+` is unsupported between objects of type `Unknown | int | None` and `int`

pywin32 (https://github.com/mhammond/pywin32)
+ error[invalid-argument-type] Pythonwin/pywin/framework/editor/configui.py:254:41: Argument to function `RGB` is incorrect: Expected `int`, found `Unknown | Literal["Black", "Navy", "Green", "Cyan", "Maroon", "Purple", "Olive", "Gray", "Silver", "Blue", "Lime", "Aqua", "Red", "Fuchsia", "Yellow", "White", 0, 128, 192, 255]`
+ error[invalid-argument-type] Pythonwin/pywin/framework/editor/configui.py:254:47: Argument to function `RGB` is incorrect: Expected `int`, found `Unknown | Literal["Black", "Navy", "Green", "Cyan", "Maroon", "Purple", "Olive", "Gray", "Silver", "Blue", "Lime", "Aqua", "Red", "Fuchsia", "Yellow", "White", 0, 128, 192, 255]`
+ error[invalid-argument-type] Pythonwin/pywin/framework/editor/configui.py:254:53: Argument to function `RGB` is incorrect: Expected `int`, found `Unknown | Literal["Black", "Navy", "Green", "Cyan", "Maroon", "Purple", "Olive", "Gray", "Silver", "Blue", "Lime", "Aqua", "Red", "Fuchsia", "Yellow", "White", 0, 128, 192, 255]`
+ error[call-non-callable] com/win32com/makegw/makegwparse.py:786:16: Object of type `int` is not callable
- error[invalid-argument-type] com/win32com/test/testDynamic.py:61:51: Argument to function `Dispatch` is incorrect: Expected `str | PyIDispatch | tuple[@Todo(Generic tuple specializations), ...] | PyIUnknown`, found `Unknown | PyIID`
+ error[invalid-argument-type] com/win32com/test/testDynamic.py:61:51: Argument to function `Dispatch` is incorrect: Expected `str | PyIDispatch | tuple[type[str], <class 'PyIID'>] | PyIUnknown`, found `Unknown | PyIID`
+ error[invalid-argument-type] com/win32comext/adsi/demos/scp.py:393:45: Argument to function `wrap` is incorrect: Expected `str`, found `Unknown | str | None`
- error[invalid-argument-type] win32/Demos/win32gui_menu.py:434:67: Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[Any, Any, Any, Any]`
+ error[invalid-argument-type] win32/Demos/win32gui_menu.py:434:67: Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[@Todo(Unpack variable-length tuple), @Todo(Unpack variable-length tuple), @Todo(Unpack variable-length tuple), @Todo(Unpack variable-length tuple)]`
- Found 2318 diagnostics
+ Found 2323 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-return-type] psycopg/psycopg/_py_transformer.py:329:16: Return type does not match returned value: expected `Row`, found `tuple[@Todo(Generic tuple specializations), ...]`
+ error[invalid-return-type] psycopg/psycopg/_py_transformer.py:329:16: Return type does not match returned value: expected `Row`, found `tuple[Any, ...]`

discord.py (https://github.com/Rapptz/discord.py)
- error[invalid-argument-type] discord/ext/commands/converter.py:1122:16: Argument to function `len` is incorrect: Expected `Sized`, found `tuple[T] | T`
+ error[invalid-argument-type] discord/ext/commands/converter.py:1122:16: Argument to function `len` is incorrect: Expected `Sized`, found `(tuple[T] & tuple[Unknown, ...]) | (T & tuple[Unknown, ...]) | tuple[T] | T`
+ warning[possibly-unbound-implicit-call] discord/ext/commands/converter.py:1124:21: Method `__getitem__` of type `(tuple[T] & tuple[Unknown, ...]) | (T & tuple[Unknown, ...]) | tuple[T] | T` is possibly unbound
- warning[possibly-unbound-implicit-call] discord/ext/commands/converter.py:1124:21: Method `__getitem__` of type `tuple[T] | T` is possibly unbound
- warning[unused-ignore-comment] discord/ext/commands/converter.py:1136:75: Unused blanket `type: ignore` directive
- Found 576 diagnostics
+ Found 575 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ warning[possibly-unbound-attribute] cwltool/cwlprov/__init__.py:16:20: Attribute `split` on type `str | int` is possibly unbound
- Found 188 diagnostics
+ Found 189 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ error[invalid-argument-type] pymongo/asynchronous/auth.py:224:45: Argument to function `_canonicalize_hostname` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | Unknown | str | int`
- warning[unused-ignore-comment] pymongo/pool_shared.py:352:84: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] pymongo/pool_shared.py:472:77: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] pymongo/pool_shared.py:521:54: Argument to bound method `wrap_socket` is incorrect: Expected `str | None`, found `Unknown | str | int | None`
+ error[invalid-argument-type] pymongo/synchronous/auth.py:221:39: Argument to function `_canonicalize_hostname` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | Unknown | str | int`
- Found 528 diagnostics
+ Found 529 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[possibly-unbound-implicit-call] examples/contrib/change_upstream_proxy.py:28:34: Method `__getitem__` of type `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] examples/contrib/change_upstream_proxy.py:28:34: Method `__getitem__` of type `Unknown | tuple[Literal["http", "https", "http3", "tls", "dtls", "tcp", "udp", "dns", "quic"], tuple[str, int]] | None` is possibly unbound
+ error[call-non-callable] mitmproxy/http.py:422:13: Method `__getitem__` of type `(Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]) | (bound method dict[str, str].__getitem__(key: str, /) -> str) | (bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown)` is not callable on object of type `str | dict[str, str] | dict[Unknown, Unknown]`
- warning[unused-ignore-comment] mitmproxy/net/server_spec.py:85:34: Unused blanket `type: ignore` directive
+ error[invalid-assignment] mitmproxy/proxy/layers/http/_upstream_proxy.py:42:13: Object of type `@Todo(map_with_boundness: intersections with negative contributions) | str | int` is not assignable to attribute `sni` of type `str | None`
+ error[invalid-assignment] test/mitmproxy/addons/test_next_layer.py:374:17: Object of type `tuple[Literal["2001:db8::1"], Literal[443], Literal[0], Literal[0]] | tuple[Literal["192.0.2.1"], Literal[443]]` is not assignable to attribute `peername` of type `tuple[str, int] | None`
- Found 1847 diagnostics
+ Found 1849 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[no-matching-overload] src/_pytest/raises.py:283:20: No overload of bound method `__init__` matches arguments
- Found 655 diagnostics
+ Found 654 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ error[no-matching-overload] lib/Crypto/SelfTest/Protocol/test_SecretSharing.py:226:47: No overload of function `__new__` matches arguments
+ error[no-matching-overload] lib/Crypto/SelfTest/Protocol/test_SecretSharing.py:226:47: No overload of function `__new__` matches arguments
+ error[no-matching-overload] lib/Crypto/SelfTest/Protocol/test_SecretSharing.py:226:47: No overload of function `__new__` matches arguments
- Found 1549 diagnostics
+ Found 1552 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- warning[possibly-unbound-attribute] aiohttp/resolver.py:102:26: Attribute `get_resolver` on type `_DNSResolverManager | None` is possibly unbound
- Found 139 diagnostics
+ Found 138 diagnostics

meson (https://github.com/mesonbuild/meson)
+ warning[possibly-unbound-attribute] mesonbuild/backend/vs2010backend.py:430:56: Attribute `parents` on type `Unknown | str | Path | MachineChoice` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/backend/vs2010backend.py:531:24: Attribute `parent` on type `Unknown | str | Path | MachineChoice` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/backend/vs2010backend.py:532:87: Attribute `parent` on type `Unknown | str | Path | MachineChoice` is possibly unbound
- Found 1216 diagnostics
+ Found 1219 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[invalid-return-type] cloudinit/cmd/main.py:277:28: Return type does not match returned value: expected `tuple[int, str]`, found `DeprecationLog`
- error[invalid-return-type] cloudinit/config/cc_growpart.py:214:12: Return type does not match returned value: expected `Resizer`, found `None | (Unknown & ~AlwaysFalsy)`
+ error[invalid-return-type] cloudinit/config/cc_growpart.py:214:12: Return type does not match returned value: expected `Resizer`, found `None | (Unknown & ~AlwaysFalsy) | (ResizeGrowPart & ~AlwaysFalsy) | (ResizeGrowFS & ~AlwaysFalsy) | (ResizeGpart & ~AlwaysFalsy)`
- error[unsupported-operator] cloudinit/util.py:1331:16: Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `set[Unknown] | Unknown | None`
+ error[unsupported-operator] cloudinit/util.py:1331:16: Operator `not in` is not supported for types `str` and `None`, in comparing `str | int | bytes` with `set[Unknown] | Unknown | None`
- Found 741 diagnostics
+ Found 740 diagnostics

apprise (https://github.com/caronc/apprise)
+ warning[possibly-unbound-attribute] apprise/plugins/email/base.py:413:21: Attribute `match` on type `Unknown | Literal["Google Mail", "Yandex", "Microsoft Hotmail", "Microsoft Outlook", "Microsoft Office 365", "Yahoo Mail", "Fast Mail", "Fast Mail Extended Addresses", "Zoho Mail", "SendGrid", "163.com", "Foxmail.com", "Comcast.net", "Custom"] | Pattern[str] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] apprise/plugins/email/base.py:420:31: Attribute `get` on type `Unknown | Literal["Google Mail", "Yandex", "Microsoft Hotmail", "Microsoft Outlook", "Microsoft Office 365", "Yahoo Mail", "Fast Mail", "Fast Mail Extended Addresses", "Zoho Mail", "SendGrid", "163.com", "Foxmail.com", "Comcast.net", "Custom"] | Pattern[str] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] apprise/plugins/email/base.py:425:34: Attribute `get` on type `Unknown | Literal["Google Mail", "Yandex", "Microsoft Hotmail", "Microsoft Outlook", "Microsoft Office 365", "Yahoo Mail", "Fast Mail", "Fast Mail Extended Addresses", "Zoho Mail", "SendGrid", "163.com", "Foxmail.com", "Comcast.net", "Custom"] | Pattern[str] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] apprise/plugins/email/base.py:431:33: Attribute `get` on type `Unknown | Literal["Google Mail", "Yandex", "Microsoft Hotmail", "Microsoft Outlook", "Microsoft Office 365", "Yahoo Mail", "Fast Mail", "Fast Mail Extended Addresses", "Zoho Mail", "SendGrid", "163.com", "Foxmail.com", "Comcast.net", "Custom"] | Pattern[str] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] apprise/plugins/email/base.py:434:40: Attribute `get` on type `Unknown | Literal["Google Mail", "Yandex", "Microsoft Hotmail", "Microsoft Outlook", "Microsoft Office 365", "Yahoo Mail", "Fast Mail", "Fast Mail Extended Addresses", "Zoho Mail", "SendGrid", "163.com", "Foxmail.com", "Comcast.net", "Custom"] | Pattern[str] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] apprise/plugins/email/base.py:442:21: Attribute `get` on type `Unknown | Literal["Google Mail", "Yandex", "Microsoft Hotmail", "Microsoft Outlook", "Microsoft Office 365", "Yahoo Mail", "Fast Mail", "Fast Mail Extended Addresses", "Zoho Mail", "SendGrid", "163.com", "Foxmail.com", "Comcast.net", "Custom"] | Pattern[str] | dict[Unknown, Unknown]` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:369:24: Attribute `parse_url` on type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:370:24: Attribute `parse_url` on type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:371:24: Attribute `parse_url` on type `Unknown | None` is possibly unbound
- Found 4453 diagnostics
+ Found 4462 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:564:25: Attribute `code` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:564:55: Attribute `filename` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:564:73: Attribute `lineno` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:569:25: Attribute `code` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:570:35: Attribute `filename` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:570:53: Attribute `lineno` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:577:31: Attribute `options` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:594:21: Attribute `code` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:597:28: Attribute `lineno` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:604:21: Attribute `filename` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/ext/doctest.py:605:21: Attribute `lineno` on type `TestCode | None` is possibly unbound
+ warning[possibly-unbound-attribute] sphinx/util/docfields.py:479:13: Attribute `append` on type `Field | list[@Todo(Support for `typing.TypeAlias`)] | Node | (Unknown & ~None)` is possibly unbound
- Found 631 diagnostics
+ Found 643 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ error[invalid-assignment] src/bokeh/layouts.py:587:13: Object of type `UIElement | int` is not assignable to `row | col`
+ error[invalid-argument-type] src/bokeh/layouts.py:588:33: Argument to function `_has_auto_sizing` is incorrect: Expected `LayoutDOM`, found `row | col`
+ error[invalid-assignment] src/bokeh/layouts.py:589:17: Object of type `Unknown & ~None` is not assignable to attribute `sizing_mode` on type `row | col`
- error[invalid-return-type] src/bokeh/layouts.py:666:16: Return type does not match returned value: expected `list[L]`, found `list[L | list[L]]`
+ error[invalid-argument-type] src/bokeh/server/tornado.py:419:31: Argument to function `issubclass` is incorrect: Expected `type`, found `str | type[RequestHandler] | dict[str, Any]`
- error[invalid-argument-type] src/bokeh/util/deprecation.py:70:10: Argument to function `warn` is incorrect: Expected `str`, found `str | (tuple[@Todo(Generic tuple specializations), ...] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/bokeh/util/deprecation.py:70:10: Argument to function `warn` is incorrect: Expected `str`, found `str | (tuple[int, int, int] & ~tuple[Unknown, ...]) | (str & ~tuple[Unknown, ...])`
- Found 920 diagnostics
+ Found 923 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[unused-ignore-comment] static_frame/core/container_util.py:1653:84: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/container_util.py:1662:81: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] static_frame/core/container_util.py:1667:39: Attribute `index_types` on type `IndexBase | IMTOAdapter | Unknown` is possibly unbound
+ warning[unused-ignore-comment] static_frame/core/db_util.py:389:66: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] static_frame/core/db_util.py:391:87: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/reduce.py:460:48: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/reduce.py:484:48: Unused blanket `type: ignore` directive
+ error[invalid-assignment] static_frame/test/unit/test_archive_npy.py:174:21: Too many values to unpack: Expected 2
- error[invalid-argument-type] static_frame/test/unit/test_store_config.py:13:31: Argument to function `label_encode_tuple` is incorrect: Expected `tuple[Any]`, found `tuple[Unknown, ...]`
- error[invalid-argument-type] static_frame/test/unit/test_store_config.py:14:31:...*[Comment body truncated]*

---

_Label `ty` added by @AlexWaygood on 2025-06-09 21:15_

---

_Comment by @AlexWaygood on 2025-06-09 21:45_

This is really exciting! One nit:

> This adds support for homogeneous tuples (`tuple[int, ...]`)

I thought we already supported homogenous tuples following https://github.com/astral-sh/ruff/pull/17998 — I thought it was just the mixed partially-homogenous-partially-heterogeneous ones that we didn't support yet?

---

_Comment by @dcreager on 2025-06-10 14:11_

> I thought we already supported homogenous tuples following #17998 — I thought it was just the mixed partially-homogenous-partially-heterogeneous ones that we didn't support yet?

Yep you're right! I had just put in a placeholder PR body for now

---

_Comment by @codspeed-hq[bot] on 2025-06-10 18:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ftuple-spec?runnerMode=Instrumentation)

### Merging #18600 will **degrade performances by 11.29%**

<sub>Comparing <code>dcreager/tuple-spec</code> (e5aa429) with <code>main</code> (d926628)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 36` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_micro[many_tuple_assignments] `` | 180.7 ms | 203.7 ms | -11.29% |


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:105 on 2025-06-10 20:21_

There were a lot of ecosystem failures of the form

```
Index 8 is out of bounds for tuple `tuple[Any, ...]` with length at least 0
```

I've fixed this in 2e41487de38c874efe06e0cf3e798f8295adebb5: indexing into a homogeneous or mixed tuple returns the variable-length portion if you index past any fixed-length prefix or suffix.

But I'm not convinced that's correct, since the spec says that a variable-length tuple is the _union_ of all possible lengths, not the gradual choice of them. That would suggest that we should treat this as a potential out-of-bounds error, since the index isn't valid for all possible elements (i.e. lengths) of the "union".

---

_@dcreager reviewed on 2025-06-10 20:23_

---

_Comment by @dcreager on 2025-06-12 13:39_

This introduces a new cycle failure in the ecosystem check for `mypy`, since this [recursive type alias][1] is now visible:

```py
SnapshotItem: _TypeAlias = tuple[Union[Primitive, "SnapshotItem"], ...]
```

[1]: https://github.com/python/mypy/blob/8241059c14f99ad750ae3ac0de6a4795bf990f61/mypy/server/astdiff.py#L118

( :tophat:  to Claude Code for helping me analyze the log files to find it more quickly)

---

_Comment by @dcreager on 2025-06-12 15:48_

There's a non-trivial performance regression for `many_tuple_assignments`. I'm inclined to accept the regression, since (a) we're genuinely doing more work, and (b) that benchmark is focused more on a combinatorial explosion of tuples in a union (https://github.com/astral-sh/ty/issues/362), and this PR is not changing that combinatorial behavior.

But I'd love a second opinion on that before I actually acknowledge the regression in Codspeed!

---

_Marked ready for review by @dcreager on 2025-06-12 15:48_

---

_Review requested from @carljm by @dcreager on 2025-06-12 15:48_

---

_Review requested from @AlexWaygood by @dcreager on 2025-06-12 15:48_

---

_Review requested from @sharkdp by @dcreager on 2025-06-12 15:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:105 on 2025-06-12 15:49_

(And if we do go with this behavior, the result for index-past-prefix might need to be the union of the variable-length portion and each element of the suffix, since we don't know for certain which part any particular index will land in.)

---

_@dcreager reviewed on 2025-06-12 15:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/literal_string.md`:61 on 2025-06-12 16:08_

?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1724 on 2025-06-12 16:16_

what do we think the type of `e.__class__()` is?

For this snippet:

```py
def f(x: tuple[int, int]):
    reveal_type(x.__class__())
```

Pyright incorrectly says `tuple[int, int]`.

Mypy does something more defensible here: it says:

```
main.py:2: error: Cannot instantiate type "Type[tuple[int, int]]"  [misc]
```

and reveals `Any`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:34 on 2025-06-12 16:18_

The problem isn't limited to legacy type aliases: we also need to be able to preserve some knowledge of the type parameters for things like

```py
class Foo(tuple[int, int]): ...
```

which is something that's on my monthly goals, since a `NamedTuple` is really just an easy way of creating a class like ^that but that has some bells and whistles attached. And `NamedTuple`s are pretty popular!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:99 on 2025-06-12 16:22_

Pyright reveals `str | Literal[1, 2, 9, 10]` here, which feels overly broad, but `str` feels incorrectly precise. What if the `*tuple[str, ...]` part here is zero-length? Wouldn't `str | Literal[9]` be the best type here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:100 on 2025-06-12 16:23_

Similarly here, wouldn't this be better as `str | Literal[9, 10]`? If the variadic middle element is length 0, it'll be `Literal[10]`; if it's length 1, it'll be `Literal[9]`; and if it's length >=2, it'll be `str`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:105 on 2025-06-12 16:26_

> But I'm not convinced that's correct, since the spec says that a variable-length tuple is the _union_ of all possible lengths, not the gradual choice of them. That would suggest that we should treat this as a potential out-of-bounds error, since the index isn't valid for all possible elements (i.e. lengths) of the "union".

That's an interesting point, but it's not what we'd do for e.g. `x[0]` if `x` is a `list[int]`, right? A very strict type checker might require you to narrow the type of `x` to one where you know the length is at least 1 before letting you do that, but in general this is an area where Python type checkers have aimed for usability.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:138 on 2025-06-12 16:27_

Can you add a TODO that says we need to preserve the type parameters better here in due course and track whether the class actually inherited from a heterogeneous tuple?

---

_@AlexWaygood reviewed on 2025-06-12 16:29_

(Only reviewed the tests so far!)

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/literal_string.md`:61 on 2025-06-12 16:58_

Gah! This is how I selectively run a single mdtest.  Removed.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1724 on 2025-06-12 17:00_

`tuple[Literal[42], ...]`

Right now, tuple _instances_ track detailed mixed element information, but I tried to keep the `tuple` class literal (and generic aliases of it) consistent with its typeshed definition, which has a single typevar.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:34 on 2025-06-12 17:10_

I think `NamedTuple` is a slight red herring here (and so are other subclasses of `tuple`).  That is, we don't have to track the more detailed tuple information here for `NamedTuple`s to be useful.

If we handle the former by collapsing it to the latter:

```py
class MyNamedTuple(tuple[int, str]):
    i: int
    s: str

class MyNamedTuple(tuple[int | str, ...]):
    i: int
    s: str
```

that wouldn't preclude you from accessing each element _by name_ and getting its correct precise type.  It would just mean that accessing elements _by (literal) index_ would collapse down to the union of all elements.  (That would also be true for a non-literal index, but that is more arguably the correct behavior.)

Now, I'm not trying to argue strongly that I _like_ this behavior. But...it is what the typeshed says that `tuple.__getitem__` does!

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:99 on 2025-06-12 17:11_

Yes I agree with this, will update the indexing impl accordingly

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:105 on 2025-06-12 17:13_

I think we can also rely on the fact that an out-of-bounds index will raise an exception. So _if the subscripting returns_, then this is the type that the result will have (modulo the comments above to include some of the suffix types as well).

---

_@dcreager reviewed on 2025-06-12 17:13_

---

_@dcreager reviewed on 2025-06-12 17:17_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:34 on 2025-06-12 17:17_

(Arguing against myself: why should an instance of `tuple[int, str]` have the nicer more precise indexing behavior, but an instance of a subclass of it not?)

---

_@dhruvmanila reviewed on 2025-06-12 17:23_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:220 on 2025-06-12 17:23_

This should only expand tuples of known length, so `tuple[X | Y, ...]` shouldn't be expanded to `tuple[X, ...], tuple[Y, ...]`. Maybe we could add a test case in this file at the end to confirm that this expansion doesn't take place now that we have variable length tuple support.

---

_@dcreager reviewed on 2025-06-12 18:40_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:220 on 2025-06-12 18:40_

Oooh great catch.  Added test and fixed

---

_Comment by @dcreager on 2025-06-13 15:32_

Moving this back to draft while I implement tracking the detail tuple element info through value forms

---

_Converted to draft by @dcreager on 2025-06-13 15:32_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1724 on 2025-06-19 21:14_

We now reveal this as `tuple[Literal[42], Literal[42]]`, tracking the full detail of the tuple elements even in the generic alias that we create for the tuple's class

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:105 on 2025-06-19 21:15_

Updated to include elements from the prefix/suffix as appropriate.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:99 on 2025-06-19 21:15_

Updated with some additional examples

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:100 on 2025-06-19 21:15_

ditto

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:34 on 2025-06-19 21:15_

We now track the full element detail in the generic aliases

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`:138 on 2025-06-19 21:16_

TODO no longer needed, this now does as you suggest 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:551 on 2025-06-19 21:20_

The newest changes on this PR now technically make the `Type::Tuple` variant redundant, since we can represent that as a `NominalInstance` of an appropriate `tuple` generic alias. I experimented with actually removing that, but there are a _lot_ of places where we have special-case logic that would have to move into `NominalInstance`, and it also seemed to introduced a bad performance regression. So I'm leaving the dedicated `Type::Tuple` variant in place for now. We can investigate removing it in a future PR.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:276 on 2025-06-19 21:20_

The primary way we're doing this is by adding an optional `TupleType` to `Specialization`. Iff the specialization is of the `tuple` class, this is the field that holds the full detail of the tuple elements. By storing it in `Specialization`, it automatically gets threaded through wherever it needs to be — generic aliases, instances of them, `type[X]`es of them, etc.


---

_@dcreager reviewed on 2025-06-19 21:20_

Sheesh, finally! That was a slog, but we're now tracking the full element detail through the specializations of the `tuple` class.

---

_Marked ready for review by @dcreager on 2025-06-19 21:22_

---

_Review requested from @MichaReiser by @dcreager on 2025-06-19 21:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/property_tests/type_generation.rs`:3 on 2025-06-20 05:45_

Nit: Should we re-export this type from `types` to reduce some of those import-only diffs?

---

_@MichaReiser reviewed on 2025-06-20 05:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:251 on 2025-06-20 05:52_

I don't understand what the meaning of `PPPP` and `SSSS` is supposed to be

---

_@MichaReiser reviewed on 2025-06-20 05:52_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/tuple.rs`:639 on 2025-06-20 06:00_

I wonder if we should remove `TupleType` and instead intern `FixedLengthTuple` and `VariableLengthTuple` separately. This has the advantage that each type gets its own interning table (we don't waste memory for the smaller variant). 

I haven't checked if `Tuple` is used as any salsa query argument. if it is, you can then derive `salsa::Supertype` on the enum to make it an "ingredient" (Note, this requires that each variant wraps a unique type, e.g. no two variant are allowed to wrap `FixedLengthTuple`)

```rust
#[derive(salsa::Supertype)]
enum TupleType<'db> {
    Fixed(FixedLengthTuple<'db>),
    Variable(VariableLengthTuple<'db>),
}

#[salsa::interned]
struct FixedLengthTuple {
    ...
}

#[salsa::interned]
struct VariableLengthTuple {
    ...
}
```

Note: I'm actually not sure if `salsa::Supertype` is a derive or a "regular" proc macro.



---

_@MichaReiser reviewed on 2025-06-20 06:00_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/tuple.rs`:639 on 2025-06-20 06:05_

The only downside of this is that you could no longer have any `&mut` methods

---

_@MichaReiser reviewed on 2025-06-20 06:05_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:625 on 2025-06-20 06:07_

Is this worth caching considering that `Tuple::homogenous` more or less directly interns the tuple (which does caching). Especially considering that the query requires interning its `Type` and dummy argument. 

---

_@MichaReiser reviewed on 2025-06-20 06:07_

---

_@MichaReiser reviewed on 2025-06-20 06:08_

I did a salsa-centric review. I leave the semantics to someone who actually understands what's changing here ;)

---

_@AlexWaygood reviewed on 2025-06-20 11:04_

The point I raised in https://github.com/astral-sh/ruff/pull/18600#discussion_r2143160777 still stands: it's unsound to allow `tuple` generic aliases to be directly instantiated without checking the arguments passed in. Mypy gets this right, pyright gets it wrong, and this branch currently makes the same mistakes as pyright here -- it reveals `tuple[int, int]` for both `reveal_type` calls without complaining about them:

```py
reveal_type(tuple[int, int]())

def f(x: tuple[int, int]):
    reveal_type(x.__class__())
```

At runtime, both calls create an object of type `tuple[()]`, _not_ an object of type `tuple[int, int]`.

It's okay to defer this for now, but it would be great to have a TODO comment for it somewhere if so!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/tuples.md`:25 on 2025-06-20 11:13_

this is so cool!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:416 on 2025-06-20 11:32_

Another test you could include is:

```py
static_assert(
    not is_assignable_to(
        tuple[Literal[1], Literal[2], *tuple[int, ...], Literal[10]],
        tuple[Literal[1], Literal[2], int, Literal[10]],
    )
)
```

That test is quite interesting because it succeeds or fails depending on whether `...` in a tuple type expression is a gradual form or not. The typing spec [states](https://typing.python.org/en/latest/spec/glossary.html#term-gradual-form) that `...` is _only_ a gradual form in the context of `tuple[Any, ...]`: this has the consequence that `tuple[Any, ...]` [should be assignable](https://typing.python.org/en/latest/spec/tuples.html#tuple-type-form) to `tuple[Any]`, since the `tuple[Any, ...]` could materialize to `tuple[Any]`. But in all other contexts, the spec states that `...` is not a gradual form in tuple type expressions: `tuple[int, ...]` should _not_ be assignable to `tuple[int]`, because it's not a gradual type. `tuple[int, ...]` expresses "the union of all fixed-length `tuple` types that only have `int` elements" rather than "a gradual type that could materialize to any fixed-length `tuple` type that only has `int` elements".

Relatedly: if we don't already, we should have some tests specifically exercising the special case that the spec carves out here for `tuple[Any, ...]`. (In our model, I think we should probably apply the same treatment to `tuple[Unknown, ...]` and `tuple[@Todo, ...]`.

---

_@AlexWaygood reviewed on 2025-06-20 11:33_

---

_Comment by @AlexWaygood on 2025-06-20 11:39_

> This introduces a new cycle failure in the ecosystem check for `mypy`, since this [recursive type alias](https://github.com/python/mypy/blob/8241059c14f99ad750ae3ac0de6a4795bf990f61/mypy/server/astdiff.py#L118) is now visible:
> 
> ```python
> SnapshotItem: _TypeAlias = tuple[Union[Primitive, "SnapshotItem"], ...]
> ```
> 
> ( 🎩 to Claude Code for helping me analyze the log files to find it more quickly)

I was somewhat surprised that this PR didn't cause us to crash on typeshed, given that typeshed uses a recursive type alias for `isinstance()` and `issubclass()`:

https://github.com/astral-sh/ruff/blob/7982edac9009e44f4928a44bde2c30216975616c/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L1527-L1534

But I guess the reason is that we still infer the `|` union as a value form there, so the subtle difference between that and mypy's alias saves us from attempting to infer the recursive alias?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:368 on 2025-06-20 11:45_

is it worth it/possible to use a smallvec for `prefix`/`suffix`? They're unlikely to be very large usually.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:625 on 2025-06-20 11:53_

If we decide that we don't need the caching, I think I'd prefer to get rid of this method entirely; a version without it feels easier to understand to me:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index cd8e8009be..0a555f811b 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -621,14 +621,6 @@ impl<'db> Type<'db> {
         matches!(self, Type::Dynamic(_))
     }
 
-    pub(crate) fn tuple_of(self, db: &'db dyn Db) -> &'db Tuple<'db> {
-        #[salsa::tracked(returns(ref))]
-        fn tuple_of<'db>(_db: &'db dyn Db, ty: Type<'db>, _dummy: ()) -> Tuple<'db> {
-            Tuple::homogeneous(ty)
-        }
-        tuple_of(db, self, ())
-    }
-
     /// Returns the top materialization (or upper bound materialization) of this type, which is the
     /// most general form of the type that is fully static.
     #[must_use]
diff --git a/crates/ty_python_semantic/src/types/generics.rs b/crates/ty_python_semantic/src/types/generics.rs
index bbd069f3d9..77d357d94b 100644
--- a/crates/ty_python_semantic/src/types/generics.rs
+++ b/crates/ty_python_semantic/src/types/generics.rs
@@ -278,14 +278,14 @@ pub struct Specialization<'db> {
 
 impl<'db> Specialization<'db> {
     /// Returns the tuple spec for a specialization of the `tuple` class.
-    pub(crate) fn tuple(self, db: &'db dyn Db) -> &'db Tuple<'db> {
+    pub(crate) fn tuple(self, db: &'db dyn Db) -> Cow<'db, Tuple<'db>> {
         if let Some(tuple) = self.tuple_inner(db).map(|tuple_type| tuple_type.tuple(db)) {
-            return tuple;
+            return Cow::Borrowed(tuple);
         }
         if let [element_type] = self.types(db) {
-            return element_type.tuple_of(db);
+            return Cow::Owned(Tuple::homogeneous(*element_type));
         }
-        Type::unknown().tuple_of(db)
+        Cow::Owned(Tuple::homogeneous(Type::unknown()))
     }
 
     /// Returns the type that a typevar is mapped to, or None if the typevar isn't part of this
diff --git a/crates/ty_python_semantic/src/types/instance.rs b/crates/ty_python_semantic/src/types/instance.rs
index 07b4f6e112..d709f9397a 100644
--- a/crates/ty_python_semantic/src/types/instance.rs
+++ b/crates/ty_python_semantic/src/types/instance.rs
@@ -19,7 +19,7 @@ impl<'db> Type<'db> {
                 TupleType::homogeneous(db, Type::unknown())
             }
             (ClassType::Generic(alias), Some(KnownClass::Tuple)) => {
-                Self::tuple(db, TupleType::new(db, alias.specialization(db).tuple(db)))
+                Self::tuple(db, TupleType::new(db, &*alias.specialization(db).tuple(db)))
             }
             _ if class.class_literal(db).0.is_protocol(db) => {
                 Self::ProtocolInstance(ProtocolInstanceType::from_class(class))
```

If we _do_ keep this method, I think it would be great to add a doc-comment describing what it does

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1 on 2025-06-20 11:56_

```suggestion
//! Types describing fixed- and variable-length tuples.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:6 on 2025-06-20 11:57_

```suggestion
//! "homogeneous" tuples that have an unknown number of elements of the same single type. And in
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 12:02_

This means that we iterate over a tuple's fixed elements twice when constructing it: once in the `TupleType` construction, and once here. We could probably avoid iterating over the tuple twice by doing this check in `VariableLengthTuple::mixed()` and `FixedLengthTuple::from_elements()`, then change this check here into a `debug_assert!()` that ensures that the inner constructors upheld the invariant.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:179 on 2025-06-20 12:21_

it feels a bit silly to have two methods that do exactly the same thing -- can't we merge `fixed_elements()` and `all_elements()` into a single `elements()` method? It feels self-evident that they will return the same thing for a `FixedLengthTuple` instance 😆

could we also maybe rename `as_slice()` to `elements_slice()`, to more clearly indicate that it returns a slice of the underlying elements?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:506 on 2025-06-20 12:43_

I don't think a variable-length tuple is ever assignable to a fixed-length tuple _except_ for the exception the spec carves out for `tuple[Any, ...]` that I mentioned in https://github.com/astral-sh/ruff/pull/18600#discussion_r2158745057. And a variable-length tuple is _never_ a subtype of a fixed-length tuple.

What's the purpose of this branch? If I replace it all with this locally (which I think is correct), no tests seem to fail:

```suggestion
        match other {
            Tuple::Fixed(_) => false,
```

If we _do_ need to keep this branch, I think you need to fixup your comment :-)

```suggestion
        match other {
            Tuple::Fixed(other) => {
                // The other tuple must have enough elements to match up with this tuple's prefix
                // and suffix, and each of those elements must pairwise satisfy the relation.
                let mut other_iter = other.0.iter();
                for self_ty in &self.prefix {
                    let Some(other_ty) = other_iter.next() else {
                        return false;
                    };
                    if !self_ty.has_relation_to(db, *other_ty, relation) {
                        return false;
                    }
                }
                for self_ty in self.suffix.iter().rev() {
                    let Some(other_ty) = other_iter.next_back() else {
                        return false;
                    };
                    if !self_ty.has_relation_to(db, *other_ty, relation) {
                        return false;
                    }
                }

                // In addition, the variable-length portion of this tuple must satisfy
                // any remaining elements in the other tuple
                other_iter.all(|other_ty| self.variable.has_relation_to(db, *other_ty, relation))
            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:776 on 2025-06-20 12:47_

The invariant here seems to be that two "pure homogeneous tuples" `tuple[A, ...]` and `tuple[B, ...]` (both tuples without prefixes and suffixes) can never be disjoint even if `A` and `B` are disjoint, because `tuple[Never, ...]` (== `tuple[()]`) would be assignable to both? That might be useful to add as a comment!

---

_Renamed from "[ty] [WIP] Homogeneous and mixed tuples" to "[ty] Homogeneous and mixed tuples" by @dcreager on 2025-06-20 12:48_

---

_@AlexWaygood reviewed on 2025-06-20 12:50_

This looks excellent overall!! I haven't reviewed the changes to `unpacker.rs` and `util/subscript.rs` in any depth

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:639 on 2025-06-20 12:59_

> The only downside of this is that you could no longer have any `&mut` methods

Yep, this part is important to keep, with how we're building up tuples during type inference and unpacking.

> we don't waste memory for the smaller variant

I had actually considered using a `SmallVec` in `FixedLengthTuple` to make it take up roughly the same amount of space as `VariableLengthTuple`.


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/property_tests/type_generation.rs`:3 on 2025-06-20 13:03_

[sad trombone] I had found that pattern confusing, tbh, since it means that we're importing types from somewhere other than where they're actually defined.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/display.rs`:251 on 2025-06-20 13:08_

Added an explanation to the comment

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:368 on 2025-06-20 13:16_

I had thought that wouldn't be important to optimize, since this will typically be hidden behind a salsa-interned `TupleType` anyway, so an extra heap allocation won't be that bad.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-20 13:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:111 on 2025-06-20 13:40_

I don't _love_ having an `is_disjoint_from_class` method because a `ClassType` isn't really a _type_ (despite its name) -- whether or not a `NominalInstanceType` is "disjoint from a ClassType" depends on whether the ClassType is wrapped in a `Type::NominalInstance` variant, a `Type::SubclassOf` variant or something else entirely. The signature of `NominalInstanceType::is_disjoint_from()` expresses this clearly, but the signature of the new method doesn't

---

_@AlexWaygood reviewed on 2025-06-20 13:42_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 16:50_

> by doing this check in `VariableLengthTuple::mixed()` and `FixedLengthTuple::from_elements()`

The issue with that is that there will be some tuple specs (for unpacking and splatting) where I think we _don't_ want to collapse a tuple containing `Never` into `Never` itself.  I don't anticipate tuples will be large enough to make that double iteration an bottleneck.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:625 on 2025-06-20 16:52_

I had the `Cow` version originally and then did this to try to remove it! I ended up with a version that doesn't need `Cow` _or_ the `tuple_of` function. As @MichaReiser points out, `TupleType` itself does the caching that we need to always be able to return a `&'db` ref.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:179 on 2025-06-20 16:55_

Done! I had the two copies so that both tuple variants has methods that aligned, but that's not really necessary

---

_@dcreager reviewed on 2025-06-20 16:56_

Some initial responses

---

_Comment by @carljm on 2025-06-20 17:00_

Since @MichaReiser did a Salsa-focused review and @AlexWaygood did a semantics-focused review, I'll consider that good enough! Let me know if there's anything in particular you want me to look at here.

---

_Review request for @carljm removed by @carljm on 2025-06-20 17:00_

---

_@AlexWaygood reviewed on 2025-06-20 17:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:368 on 2025-06-20 17:07_

SG, we can leave this unless it shows up in profiles later

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 17:09_

> The issue with that is that there will be some tuple specs (for unpacking and splatting) where I think we _don't_ want to collapse a tuple containing `Never` into `Never` itself.

hmm... do you have any examples? :-)

---

_@AlexWaygood reviewed on 2025-06-20 17:09_

---

_@dcreager reviewed on 2025-06-20 17:40_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 17:40_

[Here](https://github.com/astral-sh/ruff/blob/45cd117ea4a361db9041304462bca7b8be7a8a80/crates/ty_python_semantic/src/types/unpacker.rs#L281-L310) is where we're building up a new tuple spec (but not a tuple type) during unpacking. Right now that only works on fixed-length tuples, but in a later PR I want to extend that to support mixed tuples as well, and use that whole machinery for splatting. We'd use tuple specs to describe the positional parameters and arguments, and I wanted to be able to retain the information about which particular params/args are `Never` without having to collapse the whole thing down to a single `Never`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 17:45_

Or put another way, `Tuple` (the Rust type) will not only be used to represent instances of `tuple` (the Python type). That might have been too implicit in this PR? I can add some clarifying documentation about that point if so.

(I've been using "tuple spec" in my comments to mean "a sequence of types, which might have a homogeneous variable-length bit in the middle". A `tuple` instance is _one_ of the things that is described by a tuple spec, but not the only one. We do want the "collapse the `Never`s" behavior for `tuple` instances, but (I contend) that's not universally true of tuple specs.)

---

_@dcreager reviewed on 2025-06-20 17:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 17:48_

Ahh, thanks, that makese sense! I guess the only quibbles I have in that case are that you could maybe adjust the docstrings for `FixedLengthTuple` and `VariableLengthTuple` -- i.e., since `FixedLengthTuple` (intentionally!) does not uphold all the invariants maintained by `TupleType`, the first sentence is not _really_ accurate:

> /// A fixed-length tuple.

It would be _more_ accurate to say "Inner data for a fixed-length tuple." And maybe explicitly call out in the docstring that not all invariants we maintain for `TupleType` are maintained by this inner struct?

---

_@AlexWaygood reviewed on 2025-06-20 17:48_

---

_@AlexWaygood reviewed on 2025-06-20 17:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 17:49_

oops, I posted my response before seeing your second comment. Yes, some clarifying docs would be great! And maybe it would even be good to rename your `Tuple` struct to `TupleSpec`?

---

_@dcreager reviewed on 2025-06-20 17:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:111 on 2025-06-20 17:51_

Would it be enough to rename this to something like `is_disjoint_from_nominal_instance_of_class`?

The issue is that I need to be able to perform a disjointness check between a `tuple` instance and an instance of some other type, in the current world where `tuple` instances are not stored as `NominalInstance`s. So this was a way to reuse the existing disjointness logic without having to jump through hoops to create a `NominalInstance` for the tuple. (I'm otherwise trying very hard to enforce that it's not possible to create a `NominalInstance` of `KnownClass::Tuple`, since there is so much special-case behavior for tuples.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:111 on 2025-06-20 18:20_

Hmmm, I see. I still don't _love_ it, but I now understand the reason for it, and that name would definitely be better. Could you also add a comment encapsulating what you said above, as well -- that the method here is required so that we can preserve the invariant that tuples are always represented as `Type::Tuple`s, never as `Type::Instance`s (even "pure homogeneous" tuples)?

---

_@AlexWaygood reviewed on 2025-06-20 18:20_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:111 on 2025-06-20 18:56_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:43 on 2025-06-20 18:56_



> Yes, some clarifying docs would be great! And maybe it would even be good to rename your `Tuple` struct to `TupleSpec`?

Done

---

_@dcreager reviewed on 2025-06-20 18:56_

---

_Comment by @AlexWaygood on 2025-06-20 21:02_

(It's fine to address this comment in a followup; doesn't need to be tackled in this PR!) --

Do we need to add some more tests and TODOs around generics solving for mixed tuples and pure-homogeneous tuples? It looks like for these four `reveal_type` calls, we're only able to solve the `TypeVar` for the call to `g()` on this branch -- the rest of the calls are all revealed as `Unknown`:

```py
from typing import reveal_type

def f[T](x: tuple[int, bytes, *tuple[str, ...], T, int]) -> T:
    return x[-2]

reveal_type(f((1, b"foo", "bar", "baz", True, 42)))  # Unknown

def f2[T](x: tuple[int, T, *tuple[str, ...], bool, int]) -> T:
    return x[1]

reveal_type(f2((1, b"foo", "bar", "baz", True, 42)))  # Unknown

def g[T](x: tuple[T, int]) -> T:
    return x[0]

reveal_type(g((True, 42)))  # bool

def h[T](x: tuple[T, ...]) -> T:
    return x[0]

reveal_type(h((42,)))  # Unknown
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:776 on 2025-06-20 21:32_

Err, actually I was just going with "false negatives are okay if you haven't thought about this case yet" :sweat_smile: 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:506 on 2025-06-20 21:32_

I added several other test cases (including for the `tuple[Any, ...]` carveout) and updated this branch accordingly

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:416 on 2025-06-20 21:39_

Added these all in a new section below (ditto for subtyping)

---

_@dcreager reviewed on 2025-06-20 21:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:471 on 2025-06-20 21:53_

it would be great to link to the specific parts of the spec that describe this special case, so that it's obvious to future readers of this test why it exists (https://typing.python.org/en/latest/spec/glossary.html#term-gradual-form, https://typing.python.org/en/latest/spec/tuples.html#tuple-type-form)

---

_Closed by @dcreager on 2025-06-20 21:54_

---

_@AlexWaygood reviewed on 2025-06-20 21:55_

---

_Reopened by @dcreager on 2025-06-20 21:55_

---

_Comment by @dcreager on 2025-06-20 21:56_

> The point I raised in [#18600 (comment)](https://github.com/astral-sh/ruff/pull/18600#discussion_r2143160777) still stands: it's unsound to allow `tuple` generic aliases to be directly instantiated without checking the arguments passed in.

I misunderstood this the first time around — I thought you were asking about `reveal_type((1, 2).__class__)`, not `reveal_type((1, 2).__class__())`. Now that I understand what you mean, I added these as TODO tests.


---

_@AlexWaygood approved on 2025-06-20 22:00_

Really great work -- thank you!

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:471 on 2025-06-20 22:00_

Done

---

_@dcreager reviewed on 2025-06-20 22:00_

---

_@AlexWaygood reviewed on 2025-06-20 22:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:776 on 2025-06-20 22:02_

I _think_ that my reasoning here nonetheless holds (for pure-homogeneous tuples, but not for mixed tuples), so you could consider adding it as a comment 😆

---

_Comment by @dcreager on 2025-06-20 22:08_

> Do we need to add some more tests and TODOs around generics solving for mixed tuples and pure-homogeneous tuples?

Done. (We already had a TODO comment in the code, just no TODO tests)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:776 on 2025-06-20 22:12_

Done

---

_@dcreager reviewed on 2025-06-20 22:12_

---

_Comment by @codspeed-hq[bot] on 2025-06-20 22:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ftuple-spec?runnerMode=WallTime)

### Merging #18600 will **improve performances by 4.04%**

<sub>Comparing <code>dcreager/tuple-spec</code> (e5aa429) with <code>main</code> (d926628)</sub>



### Summary

`⚡ 1` improvements  
`✅ 5` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` medium[colour-science] `` | 9 s | 8.7 s | +4.04% |


---

_Comment by @AlexWaygood on 2025-06-20 22:23_

There is a fairly big primer report here -- have you spot-checked them/do the new diagnostics make sense?

---

_Merged by @dcreager on 2025-06-20 22:23_

---

_Closed by @dcreager on 2025-06-20 22:23_

---

_Branch deleted on 2025-06-20 22:23_

---

_Comment by @carljm on 2025-06-21 20:47_

Another really useful follow-up to uncover any issues here would be to add support to our property tests to generate variadic and mixed tuple types. Should be pretty easy (unless it uncovers actual problems!)

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-23 06:45_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-23 06:45_

---

_Comment by @sharkdp on 2025-06-23 08:12_

> There is a fairly big primer report here

I uploaded a rich diff here: https://shark.fish/diff-tuple-spec.html

---

_Comment by @AlexWaygood on 2025-06-23 12:50_

> I uploaded a rich diff here: [shark.fish/diff-tuple-spec.html](https://shark.fish/diff-tuple-spec.html)

I have a WIP patch locally to fix the 58 new `incompatible-slots` false positives on sympy

---

_Comment by @dcreager on 2025-06-23 12:59_


> `strawberry/tools/merge_types.py`
> [error] unresolved-attribute - [:26:11](https://github.com/strawberry-graphql/strawberry/blob/0c79fb79a1673b54c2ae624b94e78ced5b8b0fee/strawberry/tools/merge_types.py#L26) - Type `type` has no attribute `__strawberry_definition__`

This one is because there's a function that returns a `TypeGuard` of a `Protocol`, where the protocol includes the attribute in question

---

_Comment by @dcreager on 2025-06-23 13:27_

A lot of the new `possibly-unbound-attribute` diagnostics are because of https://github.com/astral-sh/ty/issues/625 — there's a union with `Unknown`, which means we fall back on the typeshed definition of `tuple.__getitem__`, which returns a union of all elements instead of hitting our special-case logic to pull out a specific element.

---
