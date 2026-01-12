```yaml
number: 21139
title: "[ty] Understand legacy and PEP 695 `ParamSpec`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dhruv/paramspec
created_at: 2025-10-30T14:24:17Z
updated_at: 2025-11-06T16:14:42Z
url: https://github.com/astral-sh/ruff/pull/21139
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Understand legacy and PEP 695 `ParamSpec`

---

_@dhruvmanila_

## Summary

This PR adds support for understanding the legacy definition and PEP 695 definition for `ParamSpec`.

This is still very initial and doesn't really implement any of the semantics.

Part of https://github.com/astral-sh/ty/issues/157

## Test Plan

Add mdtest cases.

## Ecosystem analysis

Most of the diagnostics in `starlette` are due to the fact that ty now understands `ParamSpec` is not a `Todo` type, so the assignability check fails. The code looks something like:

```py
class _MiddlewareFactory(Protocol[P]):
    def __call__(self, app: ASGIApp, /, *args: P.args, **kwargs: P.kwargs) -> ASGIApp: ...  # pragma: no cover

class Middleware:
    def __init__(
        self,
        cls: _MiddlewareFactory[P],
        *args: P.args,
        **kwargs: P.kwargs,
    ) -> None:
        self.cls = cls
        self.args = args
        self.kwargs = kwargs

# ty complains that `ServerErrorMiddleware` is not assignable to `_MiddlewareFactory[P]`
Middleware(ServerErrorMiddleware, handler=error_handler, debug=debug)
```

There are multiple diagnostics where there's an attribute access on the `Wrapped` object of `functools` which Pyright also raises:
```py
from functools import wraps

def my_decorator(f):
    @wraps(f)
    def wrapper(*args, **kwds):
        return f(*args, **kwds)

	# Pyright: Cannot access attribute "__signature__" for class "_Wrapped[..., Unknown, ..., Unknown]"
      Attribute "__signature__" is unknown [reportAttributeAccessIssue]
	# ty: Object of type `_Wrapped[Unknown, Unknown, Unknown, Unknown]` has no attribute `__signature__` [unresolved-attribute]
    wrapper.__signature__
    return wrapper
```

There are additional diagnostics that is due to the assignability checks failing because ty now infers the `ParamSpec` instead of using the `Todo` type which would always succeed. This results in a few `no-matching-overload` diagnostics because the assignability checks fail.

There are a few diagnostics related to https://github.com/astral-sh/ty/issues/491 where there's a variable which is either a bound method or a variable that's annotated with `Callable` that doesn't contain the instance as the first parameter.

Another set of (valid) diagnostics are where the code hasn't provided all the type variables. ty is now raising diagnostics for these because we include `ParamSpec` type variable in the signature. For example, `staticmethod[Any]` which contains two type variables.

---

_Label `ty` added by @dhruvmanila on 2025-10-30 14:24_

---

_Comment by @github-actions[bot] on 2025-10-30 14:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-03 15:09:54.839284582 +0000
+++ new-output.txt	2025-11-03 15:09:58.086280840 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(182f5)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18371)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -146,7 +146,9 @@
 callables_protocol.py:97:1: error[invalid-assignment] Object of type `def cb4_bad1(x: int) -> None` is not assignable to `Proto4`
 callables_protocol.py:121:1: error[invalid-assignment] Object of type `def cb6_bad1(*vals: bytes, *, max_len: int | None = None) -> list[bytes]` is not assignable to `NotProto6`
 callables_protocol.py:169:1: error[invalid-assignment] Object of type `def cb8_bad1(x: int) -> Any` is not assignable to `Proto8`
-callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
+callables_protocol.py:186:5: error[invalid-assignment] Object of type `Literal["str"]` is not assignable to attribute `other_attribute` of type `int`
+callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`.
+callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[Unknown, Unknown]` has no attribute `other_attribute2`
 callables_protocol.py:238:1: error[invalid-assignment] Object of type `def cb11_bad1(x: int, y: str, /) -> Any` is not assignable to `Proto11`
 callables_protocol.py:260:1: error[invalid-assignment] Object of type `def cb12_bad1(*args: Any, *, kwarg0: Any) -> None` is not assignable to `Proto12`
 callables_protocol.py:284:1: error[invalid-assignment] Object of type `def cb13_no_default(path: str) -> str` is not assignable to `Proto13_Default`
@@ -433,7 +435,11 @@
 generics_defaults.py:59:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_defaults.py:63:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_defaults.py:79:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:80:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(specialized non-generic class)`
+generics_defaults.py:80:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+generics_defaults.py:80:32: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
+generics_defaults.py:81:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
+generics_defaults.py:81:46: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:94:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_defaults.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(specialized non-generic class)`
@@ -453,8 +459,10 @@
 generics_defaults_specialization.py:27:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, bool]`
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
+generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
+generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[@Todo(Support for `typing.ParamSpec`), ...]`
+generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[@Todo(ParamSpecArgs / ParamSpecKwargs), ...]`
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
 generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
 generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -465,7 +473,6 @@
 generics_paramspec_semantics.py:53:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:57:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:76:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-generics_paramspec_semantics.py:82:5: error[type-assertion-failure] Argument does not have asserted type `@Todo(specialized non-generic class)`
 generics_paramspec_semantics.py:82:28: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
 generics_paramspec_semantics.py:84:5: error[type-assertion-failure] Argument does not have asserted type `(int, /) -> str`
 generics_paramspec_semantics.py:87:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -480,6 +487,7 @@
 generics_paramspec_specialization.py:40:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_paramspec_specialization.py:40:31: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_paramspec_specialization.py:52:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str, bool]`?
+generics_paramspec_specialization.py:58:15: error[too-many-positional-arguments] Too many positional arguments to class `ClassC`: expected 1, got 3
 generics_scoping.py:14:1: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:42:1: error[type-assertion-failure] Argument does not have asserted type `str`
@@ -837,7 +845,6 @@
 protocols_subtyping.py:80:5: error[invalid-assignment] Object of type `Proto4[int, int]` is not assignable to `Proto5[int | float]`
 protocols_subtyping.py:102:5: error[invalid-assignment] Object of type `Proto6[int | float, int | float]` is not assignable to `Proto7[int, int | float]`
 protocols_subtyping.py:103:5: error[invalid-assignment] Object of type `Proto6[int | float, int | float]` is not assignable to `Proto7[int | float, object]`
-protocols_variance.py:85:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 qualifiers_annotated.py:38:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 qualifiers_annotated.py:39:17: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
 qualifiers_annotated.py:39:18: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
@@ -988,5 +995,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 990 diagnostics
+Found 997 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-30 14:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/task_cache.py:197:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/async_utils/task_cache.py:283:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 27 diagnostics
+ Found 25 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/util/pattern.py:92:21: error[invalid-assignment] Implicit shadowing of function `getter`
+ lib/spack/spack/vendor/jinja2/async_utils.py:41:9: error[unresolved-attribute] Unresolved attribute `jinja_async_variant` on type `_Wrapped[Unknown, Unknown, Unknown, Unknown]`.
+ lib/spack/spack/vendor/jinja2/compiler.py:56:12: error[invalid-return-type] Return type does not match returned value: expected `F@optimizeconst`, found `_Wrapped[Unknown, Unknown, Unknown, Unknown]`
- Found 7869 diagnostics
+ Found 7872 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/decorators.pyi:70:10: error[too-many-positional-arguments] Too many positional arguments to class `PureAsyncDecoratorBinder`: expected 1, got 2
+ asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `__name__`
+ asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `__name__`
+ asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, Unknown].asynq(...) -> AsyncTask[Any]`.
+ asynq/tests/test_tools.py:189:17: error[unresolved-attribute] Object of type `DecoratorBinder[Unknown]` has no attribute `__acached_per_instance_cache__`
+ asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `dirty`
+ asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `dirty`
- asynq/tools.py:336:13: warning[possibly-missing-attribute] Attribute `dirty` may be missing on object of type `DecoratorBinder[Unknown] | @Todo`
- asynq/tools.py:338:13: warning[possibly-missing-attribute] Attribute `dirty` may be missing on object of type `DecoratorBinder[Unknown] | @Todo`
+ asynq/tools.py:206:9: error[unresolved-attribute] Unresolved attribute `__acached_per_instance_cache__` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:268:17: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `alazy_constant_refresh_time`
+ asynq/tools.py:269:33: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `alazy_constant_refresh_time`
+ asynq/tools.py:271:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:272:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:273:20: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `alazy_constant_cached_value`
+ asynq/tools.py:276:13: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:278:9: error[unresolved-attribute] Unresolved attribute `dirty` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:279:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:280:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:310:9: error[unresolved-attribute] Unresolved attribute `original_fn` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:336:13: error[unresolved-attribute] Object of type `DecoratorBinder[Unknown]` has no attribute `dirty`
+ asynq/tools.py:338:13: error[unresolved-attribute] Object of type `DecoratorBinder[Unknown]` has no attribute `dirty`
+ asynq/tools.py:350:77: error[unresolved-attribute] Object of type `Self@cache_key` has no attribute `fn`
+ asynq/tools.py:353:16: error[unresolved-attribute] Object of type `Self@asyncio` has no attribute `fn`
+ asynq/tools.py:357:20: error[unresolved-attribute] Object of type `Self@asynq` has no attribute `fn`
+ asynq/tools.py:364:20: error[unresolved-attribute] Object of type `Self@asynq` has no attribute `fn`
+ asynq/tools.py:377:24: error[unresolved-attribute] Object of type `Self@asynq` has no attribute `fn`
- Found 150 diagnostics
+ Found 171 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:242:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 409 diagnostics
+ Found 408 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_util/cache/utilcachecall.py:266:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/cache/utilcachecall.py:493:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/cache/utilcacheobjattr.py:637:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 517 diagnostics
+ Found 514 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/async_utils.py:53:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/compiler.py:58:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 184 diagnostics
+ Found 182 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/execution/execute.py:216:5: error[invalid-assignment] Object of type `staticmethod[Unknown, Unknown]` is not assignable to `(Any, /) -> @Todo`
- Found 389 diagnostics
+ Found 390 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/config/compat.py:81:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/config/compat.py:82:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/config/compat.py:85:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 484 diagnostics
+ Found 481 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/applications.py:86:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ServerErrorMiddleware'>`
+ starlette/applications.py:88:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ExceptionMiddleware'>`
+ starlette/applications.py:241:33: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:84:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_base.py:152:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'aMiddleware'>`
+ tests/middleware/test_base.py:153:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'bMiddleware'>`
+ tests/middleware/test_base.py:154:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'cMiddleware'>`
+ tests/middleware/test_base.py:169:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_base.py:187:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_base.py:277:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:315:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:335:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:362:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:433:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:434:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ContextManagerMiddleware'>`
+ tests/middleware/test_base.py:609:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:638:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:667:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:693:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:725:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:754:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:843:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:871:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:1086:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'MyMiddleware'>`
+ tests/middleware/test_base.py:1220:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:1278:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'PassthroughMiddleware'>`
+ tests/middleware/test_cors.py:20:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:81:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:132:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:181:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:222:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:255:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:285:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:310:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:382:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:415:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:436:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:456:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:473:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:492:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:513:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_gzip.py:23:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:41:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:61:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:84:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:107:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:132:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:155:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:180:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_https_redirect.py:16:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'HTTPSRedirectMiddleware'>`
+ tests/middleware/test_middleware.py:16:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_middleware.py:21:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_session.py:35:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:67:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:92:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:127:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:146:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:165:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:187:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_trusted_host.py:16:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'TrustedHostMiddleware'>`
+ tests/middleware/test_trusted_host.py:44:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'TrustedHostMiddleware'>`
+ tests/test_applications.py:118:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'TrustedHostMiddleware'>`
+ tests/test_applications.py:516:28: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SimpleInitializableMiddleware'>`
+ tests/test_applications.py:517:28: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'NoOpMiddleware'>`
+ tests/test_applications.py:551:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'MiddlewareWithArgs'>`
+ tests/test_applications.py:552:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'MiddlewareWithArgs'>`
+ tests/test_applications.py:574:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `def _middleware_factory(app: @Todo, arg: str) -> @Todo`
+ tests/test_applications.py:575:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `(@Todo, str, /) -> @Todo`
+ tests/test_authentication.py:188:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'AuthenticationMiddleware'>`
+ tests/test_authentication.py:343:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'AuthenticationMiddleware'>`
+ tests/test_requests.py:645:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/test_routing.py:327:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/test_routing.py:930:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'AddHeadersMiddleware'>`
+ tests/test_routing.py:948:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'AddHeadersMiddleware'>`
+ tests/test_routing.py:965:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'AddHeadersMiddleware'>`
+ tests/test_routing.py:1069:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'NamedMiddleware'>`
+ tests/test_routing.py:1074:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'NamedMiddleware'>`
+ tests/test_routing.py:1121:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'WebsocketMiddleware'>`
+ tests/test_staticfiles.py:59:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/test_templates.py:82:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/test_testclient.py:190:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BrokenMiddleware'>`
- Found 142 diagnostics
+ Found 223 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/globals.py:251:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 80 diagnostics
+ Found 79 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_validate_call.py:44:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/dataclasses.py:268:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/deprecated/decorator.py:58:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/deprecated/decorator.py:59:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/deprecated/decorator.py:60:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/deprecated/decorator.py:61:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/decorator.py:42:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/decorator.py:43:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/decorator.py:44:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/decorator.py:45:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/main.py:935:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, @Todo]`
+ pydantic/v1/main.py:935:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, classmethod[Any, Any, Any]]`
- pydantic/v1/main.py:949:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, @Todo]`
+ pydantic/v1/main.py:949:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, classmethod[Any, Any, Any]]`
- pydantic/v1/main.py:962:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, @Todo]`
+ pydantic/v1/main.py:962:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, classmethod[Any, Any, Any]]`
+ pydantic/v1/typing.py:283:26: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
- Found 775 diagnostics
+ Found 766 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- src/check_jsonschema/cli/param_types.py:24:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 53 diagnostics
+ Found 52 diagnostics

nox (https://github.com/wntrblm/nox)
+ nox/_decorators.py:61:5: error[unresolved-attribute] Unresolved attribute `__kwdefaults__` on type `_Wrapped[Unknown, Unknown, Unknown, Unknown]`.
- Found 27 diagnostics
+ Found 28 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:274:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 331 diagnostics
+ Found 330 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/pq/_debug.py:108:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 684 diagnostics
+ Found 683 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/retrying.py:25:13: error[unresolved-attribute] Unresolved attribute `failed_count` on type `_Wrapped[Unknown, Unknown, Unknown, Unknown]`.
+ cki_lib/retrying.py:26:13: error[unresolved-attribute] Unresolved attribute `retries` on type `_Wrapped[Unknown, Unknown, Unknown, Unknown]`.
+ cki_lib/retrying.py:35:21: error[unresolved-attribute] Object of type `_Wrapped[Unknown, Unknown, Unknown, Unknown]` has no attribute `failed_count`
+ cki_lib/retrying.py:37:24: error[unresolved-attribute] Object of type `_Wrapped[Unknown, Unknown, Unknown, Unknown]` has no attribute `failed_count`
- Found 187 diagnostics
+ Found 191 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/_internal/typing.py:91:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Out@__call__`
- src/antidote/_internal/typing.py:96:70: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Out@__call__`
- src/antidote/core/__init__.py:1302:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> T@__wrapped__`
- src/antidote/core/__init__.py:1305:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@__call__`
+ src/antidote/core/__init__.py:985:41: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/__init__.py:985:61: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/core/__init__.py:1033:16: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/__init__.py:1042:10: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/__init__.py:1048:16: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/core/__init__.py:1057:10: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/core/_inject.py:93:21: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/_inject.py:93:39: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/core/_inject.py:132:16: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/_inject.py:141:10: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/_inject.py:147:16: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/core/_inject.py:156:10: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/core/_injection.py:278:53: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/core/_injection.py:278:73: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
- src/antidote/core/_injection.py:298:9: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((...) -> object) & ~staticmethod & ~classmethod) | (@Todo & ~staticmethod & ~classmethod)`
+ src/antidote/core/_injection.py:298:9: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]]) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]])`
- src/antidote/core/_injection.py:298:30: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `(((...) -> object) & ~staticmethod & ~classmethod) | (@Todo & ~staticmethod & ~classmethod)`
+ src/antidote/core/_injection.py:298:30: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]]) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]])`
- src/antidote/core/_injection.py:302:17: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((...) -> object) & ~staticmethod & ~classmethod & ~MethodType) | (@Todo & ~staticmethod & ~classmethod & ~MethodType)`
+ src/antidote/core/_injection.py:302:17: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType)`
- src/antidote/core/_injection.py:302:42: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `(((...) -> object) & ~staticmethod & ~classmethod & ~MethodType) | (@Todo & ~staticmethod & ~classmethod & ~MethodType)`
+ src/antidote/core/_injection.py:302:42: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType)`
+ src/antidote/core/_raw/onion.py:417:9: error[invalid-assignment] Object of type `ReferenceType[Self@add_child]` is not assignable to attribute `__parent_ref` of type `Function[Unknown, CatalogOnionLayerImpl | None]`
- src/antidote/core/_raw/wrapper.py:79:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/antidote/lib/interface_ext/__init__.py:576:17: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
- src/antidote/lib/interface_ext/__init__.py:687:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/interface_ext/__init__.py:708:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[(...) -> Out@single]`
- src/antidote/lib/interface_ext/__init__.py:738:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[Sequence[(...) -> Out@all]]`
- src/antidote/lib/interface_ext/__init__.py:763:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__antidote_dependency_hint__`
- src/antidote/lib/interface_ext/__init__.py:776:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/interface_ext/__init__.py:792:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[Out@__call__]`
- src/antidote/lib/interface_ext/__init__.py:800:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Dependency[Out@single]`
- src/antidote/lib/interface_ext/__init__.py:830:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Dependency[Sequence[Out@all]]`
- src/antidote/lib/interface_ext/__init__.py:1158:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/antidote/lib/interface_ext/__init__.py:1237:31: error[missing-argument] No arguments provided for required parameters `_P`, `_R_co` of class `classmethod`
+ src/antidote/lib/interface_ext/__init__.py:1241:31: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1298:23: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1299:10: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1303:32: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1303:65: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1330:23: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1331:10: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1335:32: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1335:65: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/interface_ext/__init__.py:1361:11: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
- src/antidote/lib/interface_ext/_function.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Dependency[Out@__call__]`, found `Dependency[typing.TypeVar]`
+ src/antidote/lib/interface_ext/_interface.py:257:17: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
- src/antidote/lib/lazy_ext/__init__.py:584:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/lazy_ext/__init__.py:590:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[Out@__call__]`
- src/antidote/lib/lazy_ext/__init__.py:598:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/lazy_ext/__init__.py:604:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[Out@__call__]`
+ src/antidote/lib/lazy_ext/__init__.py:210:17: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/__init__.py:216:10: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/__init__.py:499:38: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/__init__.py:505:35: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/__init__.py:635:32: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/__init__.py:635:65: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/_lazy.py:104:17: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
+ src/antidote/lib/lazy_ext/_lazy.py:110:10: error[missing-argument] No argument provided for required parameter `_R_co` of class `staticmethod`
- tests/lib/interface/test_class.py:68:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_function.py:36:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:96:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 306 diagnostics
+ Found 320 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/app_commands/commands.py:807:19: error[missing-argument] No argument provided for required parameter `error` of function `on_error`
+ discord/app_commands/commands.py:807:35: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Group`, found `Interaction[Client]`
+ discord/app_commands/commands.py:807:48: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Interaction[Client]`, found `AppCommandError`
+ discord/app_commands/commands.py:810:23: error[missing-argument] No argument provided for required parameter `error` of function `on_error`
+ discord/app_commands/commands.py:810:46: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Group`, found `Interaction[Client]`
+ discord/app_commands/commands.py:810:59: error[invalid-argument-type] Argument to function `on_error` is incorrect: Expected `Interaction[Client]`, found `AppCommandError`
+ discord/app_commands/errors.py:453:95: error[unresolved-attribute] Object of type `object` has no attribute `__qualname__`
- discord/app_commands/tree.py:754:13: error[invalid-assignment] Object of type `list[Command | Group]` is not assignable to `list[@Todo | Group | ContextMenu]`
+ discord/app_commands/tree.py:754:13: error[invalid-assignment] Object of type `list[Command[Unknown, Unknown, Unknown] | Group]` is not assignable to `list[Command[Any, @Todo, Any] | Group | ContextMenu]`
- discord/app_commands/tree.py:764:17: error[invalid-assignment] Object of type `list[Command | Group]` is not assignable to `list[@Todo | Group | ContextMenu]`
+ discord/app_commands/tree.py:764:17: error[invalid-assignment] Object of type `list[Command[Unknown, Unknown, Unknown] | Group]` is not assignable to `list[Command[Any, @Todo, Any] | Group | ContextMenu]`
+ discord/ext/commands/cog.py:288:28: error[invalid-argument-type] Argument to class `Command` is incorrect: Expected `Cog | None`, found `typing.Self`
+ discord/ext/commands/cog.py:431:16: error[invalid-return-type] Return type does not match returned value: expected `list[Command[Self@get_app_commands, @Todo, Any] | Group]`, found `list[Group | Command[typing.Self, @Todo, Any] | Unknown]`
- discord/ext/commands/cog.py:527:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:527:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` on type `(FuncT@listener & ~staticmethod[object, object]) | ((...) -> object)`
- discord/ext/commands/cog.py:528:33: error[unresolved-attribute] Object of type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)` has no attribute `__name__`
+ discord/ext/commands/cog.py:528:33: error[unresolved-attribute] Object of type `(FuncT@listener & ~staticmethod[object, object]) | ((...) -> object)` has no attribute `__name__`
- discord/ext/commands/cog.py:530:17: error[unresolved-attribute] Object of type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)` has no attribute `__cog_listener_names__`
+ discord/ext/commands/cog.py:530:17: error[unresolved-attribute] Object of type `(FuncT@listener & ~staticmethod[object, object]) | ((...) -> object)` has no attribute `__cog_listener_names__`
- discord/ext/commands/cog.py:532:17: error[invalid-assignment] Object of type `list[Unknown | (str & ~AlwaysFalsy)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:532:17: error[invalid-assignment] Object of type `list[Unknown | (str & ~AlwaysFalsy)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod[object, object]) | ((...) -> object)`
- discord/ext/commands/core.py:129:35: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/core.py:516:16: error[invalid-return-type] Return type does not match returned value: expected `CogT@cog`, found `Unknown | CogT@cog | typing.TypeVar`
- discord/ext/commands/core.py:653:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Command | @Todo`
+ discord/ext/commands/core.py:653:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Command[CogT@Command, P@Command, T@Command]`
- discord/ext/commands/core.py:660:20: error[invalid-return-type] Return type does not match returned value: expected `Self@_update_copy`, found `Command | @Todo`
+ discord/ext/commands/core.py:660:20: error[invalid-return-type] Return type does not match returned value: expected `Self@_update_copy`, found `Command[CogT@Command, P@Command, T@Command]`
+ discord/ext/commands/core.py:680:52: warning[possibly-missing-attribute] Attribute `cog_command_error` may be missing on object of type `CogT@Command & ~None`
- discord/ext/commands/core.py:815:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:820:31: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/core.py:838:39: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
+ discord/ext/commands/core.py:919:47: warning[possibly-missing-attribute] Attribute `cog_before_invoke` may be missing on object of type `CogT@Command & ~None`
+ discord/ext/commands/core.py:921:23: error[invalid-argument-type] Argument to bound method `cog_before_invoke` is incorrect: Expected `Cog`, found `CogT@Command`
- discord/ext/commands/core.py:955:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/core.py:939:47: warning[possibly-missing-attribute] Attribute `cog_after_invoke` may be missing on object of type `CogT@Command & ~None`
+ discord/ext/commands/core.py:941:23: error[invalid-argument-type] Argument to bound method `cog_after_invoke` is incorrect: Expected `Cog`, found `CogT@Command`
+ discord/ext/commands/core.py:1312:58: warning[possibly-missing-attribute] Attribute `cog_check` may be missing on object of type `CogT@Command & ~None`
- discord/ext/commands/core.py:1571:9: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/core.py:1744:91: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/core.py:1747:85: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/core.py:1949:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: @Todo) -> @Todo`.
+ discord/ext/commands/core.py:1949:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Any, @Todo, Any] | @Todo) -> Command[Any, @Todo, Any] | @Todo`.
- discord/ext/commands/core.py:1956:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: @Todo) -> @Todo`.
+ discord/ext/commands/core.py:1956:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Any, @Todo, Any] | @Todo) -> Command[Any, @Todo, Any] | @Todo`.
- discord/ext/commands/core.py:2373:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command | @Todo) -> Command | @Todo`.
+ discord/ext/commands/core.py:2373:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Unknown, Unknown, Unknown] | @Todo) -> Command[Unknown, Unknown, Unknown] | @Todo`.
- discord/ext/commands/core.py:2380:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command | @Todo) -> Command | @Todo`.
+ discord/ext/commands/core.py:2380:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Unknown, Unknown, Unknown] | @Todo) -> Command[Unknown, Unknown, Unknown] | @Todo`.
- discord/ext/commands/core.py:2448:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command | @Todo) -> Command | @Todo`.
+ discord/ext/commands/core.py:2448:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Unknown, Unknown, Unknown] | @Todo) -> Command[Unknown, Unknown, Unknown] | @Todo`.
- discord/ext/commands/core.py:2455:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command | @Todo) -> Command | @Todo`.
+ discord/ext/commands/core.py:2455:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Unknown, Unknown, Unknown] | @Todo) -> Command[Unknown, Unknown, Unknown] | @Todo`.
- discord/ext/commands/help.py:510:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/help.py:849:44: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/help.py:1199:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/help.py:1303:44: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/help.py:1589:44: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/hybrid.py:90:35: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
+ discord/ext/commands/hybrid.py:328:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `(...) -> @Todo`.
+ discord/ext/commands/hybrid.py:338:17: error[unresolved-attribute] Object of type `(...) -> @Todo` has no attribute `__signature__`
- discord/ext/commands/hybrid.py:463:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/hybrid.py:539:16: error[invalid-return-type] Return type does not match returned value: expected `CogT@cog`, found `Unknown | CogT@cog`
- discord/ext/commands/hybrid.py:603:19: error[too-many-positional-arguments] Too many positional arguments to class `Group`: expected 1, got 3
- discord/ext/commands/hybrid.py:703:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/hybrid.py:707:16: error[invalid-return-type] Return type does not match returned value: expected `CogT@cog`, found `Unknown | CogT@cog`
- Found 517 diagnostics
+ Found 514 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:692:5: error[invalid-assignment] Object of type `staticmethod[Unknown, Unknown]` is not assignable to `(str, /) -> bool`
+ mkdocs/plugins.py:490:32: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `Unknown | ((...) -> T@CombinedEvent)`
+ mkdocs/plugins.py:514:24: error[not-iterable] Object of type `object` is not iterable
- Found 220 diagnostics
+ Found 223 diagnostics

svcs (https://github.com/hynek/svcs)
+ tests/typing/starlette.py:58:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SVCSMiddleware'>`
- Found 9 diagnostics
+ Found 10 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/exceptions/conflicting_arguments.py:53:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/exceptions/invalid_argument_type.py:65:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/exceptions/missing_arguments_annotations.py:62:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/exceptions/missing_return_annotation.py:46:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/exceptions/unresolved_field_type.py:57:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/types/fields/resolver.py:392:20: error[invalid-return-type] Return type does not match returned value: expected `(...) -> T@StrawberryResolver`, found `((...) -> object) | ((...) -> Unknown) | (() -> object)`
- Found 379 diagnostics
+ Found 375 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dask/prefect_dask/task_runners.py:474:28: error[invalid-argument-type] Argument to bound method `map` is incorrect: Expected `Task[P@map, R@map | CoroutineType[Any, Any, R@map]]`, found `Task[P@map, R@map]`
+ src/integrations/prefect-kubernetes/integration_tests/src/prefect_kubernetes_integration_tests/utils/prefect_core.py:28:25: error[invalid-await] `Flow[Unknown, Any] | Coroutine[Any, Any, Flow[Unknown, Any]]` is not awaitable
- src/prefect/_internal/compatibility/async_dispatch.py:98:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/cache_policies.py:300:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & (() -> object))`
+ src/prefect/deployments/runner.py:750:59: error[unresolved-attribute] Object of type `Flow[Unknown, Any]` has no attribute `__name__`
+ src/prefect/deployments/runner.py:772:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & (() -> object))`
- src/prefect/flow_engine.py:203:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/flow_engine.py:357:20: error[invalid-return-type] Return type does not match returned value: expected `R@FlowRunEngine | State[Any] | None`, found `(R@FlowRunEngine & ~<class 'NotSet'> & ~Top[State[Any]] & ~Coroutine[object, Never, object]) | (type[NotSet] & ~<class 'NotSet'> & ~Coroutine[object, Never, object]) | Unknown | (PrefectFuture[Unknown] & ~Top[State[Any]] & ~Coroutine[object, Never, object])`
+ src/prefect/flow_engine.py:361:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)` (must be a `BaseException` subclass or instance)
+ src/prefect/flow_engine.py:362:20: error[invalid-return-type] Return type does not match returned value: expected `R@FlowRunEngine | State[Any] | None`, found `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)`
+ src/prefect/flow_engine.py:370:16: error[invalid-return-type] Return type does not match returned value: expected `R@FlowRunEngine | State[Any] | None`, found `Unknown | Coroutine[Any, Any, Unknown]`
+ src/prefect/flow_engine.py:385:9: error[invalid-assignment] Object of type `PrefectFuture[Unknown] | Any` is not assignable to attribute `_return_value` of type `R@FlowRunEngine | type[NotSet]`
+ src/prefect/flow_engine.py:448:9: error[invalid-assignment] Object of type `BaseException` is not assignable to attribute `_raised` of type `Exception | type[NotSet]`
+ src/prefect/flow_engine.py:581:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Flow[P@FlowRunEngine, R@FlowRunEngine]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:581:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:695:25: error[invalid-assignment] Object of type `int | float` is not assignable to attribute `retry_delay` of type `int | None`
- src/prefect/flow_engine.py:795:31: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `R@call_flow_fn | Coroutine[Any, Any, R@call_flow_fn]`
+ src/prefect/flow_engine.py:795:31: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]`
- src/prefect/flow_engine.py:949:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/flow_engine.py:937:20: error[invalid-return-type] Return type does not match returned value: expected `R@AsyncFlowRunEngine | State[Any] | None`, found `object`
+ src/prefect/flow_engine.py:941:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)` (must be a `BaseException` subclass or instance)
+ src/prefect/flow_engine.py:942:20: error[invalid-return-type] Return type does not match returned value: expected `R@AsyncFlowRunEngine | State[Any] | None`, found `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)`
+ src/prefect/flow_engine.py:962:9: error[invalid-assignment] Object of type `PrefectFuture[Unknown] | Any` is not assignable to attribute `_return_value` of type `R@AsyncFlowRunEngine | type[NotSet]`
+ src/prefect/flow_engine.py:1026:13: error[invalid-assignment] Object of type `BaseException` is not assignable to attribute `_raised` of type `Exception | type[NotSet]`
+ src/prefect/flow_engine.py:1158:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1158:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1276:25: error[invalid-assignment] Object of type `int | float` is not assignable to attribute `retry_delay` of type `int | None`
+ src/prefect/flow_engine.py:1473:43: error[invalid-argument-type] Argument to bound method `handle_success` is incorrect: Expected `R@run_generator_flow_sync`, found `None`
+ src/prefect/flow_engine.py:1513:49: error[invalid-argument-type] Argument to bound method `handle_success` is incorrect: Expected `R@run_generator_flow_async`, found `None`
+ src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Flow[Unknown, Unknown]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `FlowRun | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Iterable[PrefectFuture[Unknown]] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Iterable[PrefectFuture[Unknown]] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Literal["state", "result"]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1560:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Flow[Unknown, Unknown]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `FlowRun | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Iterable[PrefectFuture[Any]] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Iterable[PrefectFuture[Any]] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Literal["state", "result"]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `Flow[Unknown, Unknown]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1562:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `FlowRun | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `Iterable[PrefectFuture[Any]] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `Iterable[PrefectFuture[Any]] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `Literal["state", "result"]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1564:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1566:37: error[invalid-argument-type] Argument to function `run_flow_sync` is incorrect: Expected `Flow[Unknown, Unknown]`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1566:37: error[invalid-argument-type] Argument to function `run_flow_sync` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1566:37: error[invalid-argument-type] Argument to function `run_flow_sync` is incorrect: Expected `FlowRun | None`, found `Flow[P@run_flow, R@run_flow] | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1566:37: error[invalid-argument-type] Argument to function `run_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1566:37: error[invalid-argument-type] Argument to function `run_flow_sync` is incorrec...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-10-31 16:12_

---

_Comment by @github-actions[bot] on 2025-10-31 16:19_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 156 | 1 | 56 |
| `unresolved-attribute` | 90 | 6 | 8 |
| `unused-ignore-comment` | 0 | 58 | 0 |
| `invalid-return-type` | 20 | 22 | 3 |
| `missing-argument` | 38 | 0 | 0 |
| `invalid-assignment` | 23 | 2 | 9 |
| `possibly-missing-attribute` | 17 | 5 | 5 |
| `too-many-positional-arguments` | 0 | 14 | 0 |
| `no-matching-overload` | 10 | 0 | 0 |
| `invalid-raise` | 4 | 0 | 0 |
| `invalid-parameter-default` | 0 | 0 | 3 |
| `type-assertion-failure` | 3 | 0 | 0 |
| `call-non-callable` | 2 | 0 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `not-iterable` | 2 | 0 | 0 |
| `redundant-cast` | 0 | 2 | 0 |
| **Total** | **367** | **110** | **84** |

**[Full report with detailed diff](https://dhruv-paramspec.ecosystem-663.pages.dev/diff)** ([timing results](https://dhruv-paramspec.ecosystem-663.pages.dev/timing))


---

_Comment by @dhruvmanila on 2025-10-31 17:19_

> ```diff
> -callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
> +callables_protocol.py:186:5: error[invalid-assignment] Object of type `Literal["str"]` is not assignable to attribute `other_attribute` of type `int`
> +callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`.
> +callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[Unknown, Unknown]` has no attribute `other_attribute2`
> ```

These are correct!

> ```diff
> +generics_defaults.py:80:32: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
> ```

This will require adding special support for allowing both `Class_ParamSpec[int, str]` and `Class_ParamSpec[[int, str]]` when the class is generic over a single type variable which is a `ParamSpec`.

> ```diff
> +generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
> ```

Still need to enforce rules about where a `ParamSpec` can be used.

---

_Comment by @dhruvmanila on 2025-10-31 18:48_

I think this is a good checkpoint to get initial feedback. Follow-up work would be to:
1. Update the `Parameters` structure to accommodate `ParamSpec`
2. Representing `ParamSpecArgs`, `ParamSpecKwargs`
3. Validating the usage of `ParamSpec` e.g., it cannot be used as annotation
4. Validating the usage of `P.args` and `P.kwargs`

For (1) and (2), I think I have a good idea on how to represent them based on the following:
> A function declared as `def inner(*args: P.args, **kwargs: P.kwargs) -> X` has type `Callable[P, X]`.

And,
> The `P.args` attribute of a `ParamSpec` is an instance of `ParamSpecArgs`, and `P.kwargs` is an instance of `ParamSpecKwargs`. They are intended for runtime introspection and have no special meaning to static type checkers.

I'm thinking to create `Parameters` as `*args: P.args, **kwargs: P.kwargs` where `P.args` and `P.kwargs` would be `KnownInstanceType::ParamSpecArgs` and `KnownInstanceType::ParamSpecKwargs` respectively. Both of these variants would have a link back to the original `ParamSpec`.

---

_Marked ready for review by @dhruvmanila on 2025-10-31 18:48_

---

_Review requested from @carljm by @dhruvmanila on 2025-10-31 18:48_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-10-31 18:48_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-10-31 18:48_

---

_Review requested from @dcreager by @dhruvmanila on 2025-10-31 18:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-10-31 18:48_

---

_Comment by @AlexWaygood on 2025-10-31 21:27_

> Most of the diagnostics in `starlette` are due to the fact that ty now understands `ParamSpec` is not a `Todo` type, so the assignability check fails

There might be a short-term hack we could add to `Type::has_relation_to_impl` so that a generic type is considered to be specialized with `Unknown` if one of the typevarlikes in its generic context is a `ParamSpec`? That might reduce the false positives for users while we're still working on the full semantics for `ParamSpec`.

There aren't _that_ many new diagnostics, though, so this also might be fine for now considering that we're planning on addressing these new diagnostics shortly. Still, it's nice to avoid adding false positives for existing users where possible.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:312 on 2025-10-31 21:40_

_Ideally_ I feel like these would still be `@Todo` types, so that it's clear to users that we're aware of this limitation and that this isn't our intended final behaviour for this. But it's also fine to leave this as-is if it would be a lot of code to make these still be `@Todo` types.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:26 on 2025-10-31 21:40_

same here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/paramspec.md`:51 on 2025-10-31 21:42_

```suggestion
### `ParamSpec` parameter must match variable name
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/paramspec.md`:121 on 2025-10-31 21:43_

is it worth also explicitly testing the PEP-695 syntax where one `ParamSpec` has another `ParamSpec` as a default?

```suggestion
def foo1[**P]() -> None:
    reveal_type(P)  # revealed: typing.ParamSpec

def foo2[**P = ...]() -> None:
    reveal_type(P)  # revealed: typing.ParamSpec

def foo2[**P = [int, str]]() -> None: ...

def foo3[**P, **Q = P]():
    reveal_type(P)  # revealed: typing.ParamSpec
    reveal_type(Q)  # revealed: typing.ParamSpec
```

---

_@AlexWaygood approved on 2025-10-31 21:50_

nice

---

_@dhruvmanila reviewed on 2025-11-03 14:52_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:312 on 2025-11-03 14:52_

I believe this shouldn't be too hard, let me try...

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:312 on 2025-11-03 14:54_

Yeah, this wasn't hard, I've updated these to use `@Todo` types.

---

_@dhruvmanila reviewed on 2025-11-03 14:54_

---

_@dhruvmanila reviewed on 2025-11-03 14:54_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:26 on 2025-11-03 14:54_

And, this as well. Thanks for the suggestion!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/paramspec.md`:121 on 2025-11-03 14:56_

Yeah, I agree.

---

_@dhruvmanila reviewed on 2025-11-03 14:56_

---

_Merged by @dhruvmanila on 2025-11-06 16:14_

---

_Closed by @dhruvmanila on 2025-11-06 16:14_

---

_Branch deleted on 2025-11-06 16:14_

---
