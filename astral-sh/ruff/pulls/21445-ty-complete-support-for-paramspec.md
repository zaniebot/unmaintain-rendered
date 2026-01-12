```yaml
number: 21445
title: "[ty] Complete support for `ParamSpec`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dhruv/paramspec-args-kwargs
created_at: 2025-11-14T08:25:40Z
updated_at: 2025-12-05T16:42:29Z
url: https://github.com/astral-sh/ruff/pull/21445
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Complete support for `ParamSpec`

---

_@dhruvmanila_

## Summary

Closes: https://github.com/astral-sh/ty/issues/157

This PR adds support for the following capabilities involving a `ParamSpec` type variable:
- Representing `P.args` and `P.kwargs` in the type system
- Matching against a callable containing `P` to create a type mapping
- Specializing `P` against the stored parameters

The value of a `ParamSpec` type variable is being represented using `CallableType` with a `CallableTypeKind::ParamSpecValue` variant. This `CallableTypeKind` is expanded from the existing `is_function_like` boolean flag. An `enum` is used as these variants are mutually exclusive.

For context, an initial iteration made an attempt to expand the `Specialization` to use `TypeOrParameters` enum that represents that a type variable can specialize into either a `Type` or `Parameters` but that increased the complexity of the code as all downstream usages would need to handle both the variants appropriately. Additionally, we'd have also need to establish an invariant that a regular type variable always maps to a `Type` while a paramspec type variable always maps to a `Parameters`.

I've intentionally left out checking and raising diagnostics when the `ParamSpec` type variable and it's components are not being used correctly to avoid scope increase and it can easily be done as a follow-up. This would also include the scoping rules which I don't think a regular type variable implements either.

## Test Plan

Add new mdtest cases and update existing test cases.

Ran this branch on pyx, no new diagnostics.

### Ecosystem analysis

There's a case where in an annotated assignment like:
```py
type CustomType[P] = Callable[...]

def value[**P](...): ...

def another[**P](...):
	target: CustomType[P] = value
```
The type of `value` is a callable and it has a paramspec that's bound to `value`, `CustomType` is a type alias that's a callable and `P` that's used in it's specialization is bound to `another`. Now, ty infers the type of `target` same as `value` and does not use the declared type `CustomType[P]`. [This is the assignment](https://github.com/mikeshardmind/async-utils/blob/0980b9d9ab2bc7a24777684a884f4ea96cbbe5f9/src/async_utils/gen_transform.py#L108) that I'm referring to which then leads to error in downstream usage. Pyright and mypy does seem to use the declared type.

There are multiple diagnostics in `dd-trace-py` that requires support for `cls`.

I'm seeing `Divergent` type for an example like which ~~I'm not sure why, I'll look into it tomorrow~~ is because of a cycle as mentioned in https://github.com/astral-sh/ty/issues/1729#issuecomment-3612279974:
```py
from typing import Callable

def decorator[**P](c: Callable[P, int]) -> Callable[P, str]: ...

@decorator
def func(a: int) -> int: ...

# ((a: int) -> str) | ((a: Divergent) -> str)
reveal_type(func)
```

I ~~need to look into why are the parameters not being specialized through multiple decorators in the following code~~ think this is also because of the cycle mentioned in https://github.com/astral-sh/ty/issues/1729#issuecomment-3612279974 and the fact that we don't support `staticmethod` properly:
```py
from contextlib import contextmanager

class Foo:
    @staticmethod
    @contextmanager
    def method(x: int):
        yield

foo = Foo()
# ty: Revealed type: `() -> _GeneratorContextManager[Unknown, None, None]` [revealed-type]
reveal_type(foo.method)
```

There's some issue related to `Protocol` that are generic over a `ParamSpec` in `starlette` which might be related to https://github.com/astral-sh/ty/issues/1635 but I'm not sure. Here's a minimal example to reproduce:

<details><summary>Code snippet:</summary>
<p>

```py
from collections.abc import Awaitable, Callable, MutableMapping
from typing import Any, Callable, ParamSpec, Protocol

P = ParamSpec("P")

Scope = MutableMapping[str, Any]
Message = MutableMapping[str, Any]
Receive = Callable[[], Awaitable[Message]]
Send = Callable[[Message], Awaitable[None]]

ASGIApp = Callable[[Scope, Receive, Send], Awaitable[None]]

_Scope = Any
_Receive = Callable[[], Awaitable[Any]]
_Send = Callable[[Any], Awaitable[None]]

# Since `starlette.types.ASGIApp` type differs from `ASGIApplication` from `asgiref`
# we need to define a more permissive version of ASGIApp that doesn't cause type errors.
_ASGIApp = Callable[[_Scope, _Receive, _Send], Awaitable[None]]


class _MiddlewareFactory(Protocol[P]):
    def __call__(
        self, app: _ASGIApp, *args: P.args, **kwargs: P.kwargs
    ) -> _ASGIApp: ...


class Middleware:
    def __init__(
        self, factory: _MiddlewareFactory[P], *args: P.args, **kwargs: P.kwargs
    ) -> None:
        self.factory = factory
        self.args = args
        self.kwargs = kwargs


class ServerErrorMiddleware:
    def __init__(
        self,
        app: ASGIApp,
        value: int | None = None,
        flag: bool = False,
    ) -> None:
        self.app = app
        self.value = value
        self.flag = flag

    async def __call__(self, scope: Scope, receive: Receive, send: Send) -> None: ...


# ty: Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ServerErrorMiddleware'>` [invalid-argument-type]
Middleware(ServerErrorMiddleware, value=500, flag=True)
```

</p>
</details> 

### Conformance analysis

> ```diff
> -constructors_callable.py:36:13: info[revealed-type] Revealed type: `(...) -> Unknown`
> +constructors_callable.py:36:13: info[revealed-type] Revealed type: `(x: int) -> Unknown`
> ```

Requires return type inference i.e., https://github.com/astral-sh/ruff/pull/21551

> ```diff
> +constructors_callable.py:194:16: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
> +constructors_callable.py:194:22: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
> +constructors_callable.py:195:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | int]`
> +constructors_callable.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
> ```

I might need to look into why this is happening...

> ```diff
> +generics_defaults.py:79:1: error[type-assertion-failure] Type `type[Class_ParamSpec[(str, int, /)]]` does not match asserted type `<class 'Class_ParamSpec'>`
> ```

which is on the following code
```py
DefaultP = ParamSpec("DefaultP", default=[str, int])

class Class_ParamSpec(Generic[DefaultP]): ...

assert_type(Class_ParamSpec, type[Class_ParamSpec[str, int]])
```

It's occurring because there's no equivalence relationship defined between `ClassLiteral` and `KnownInstanceType::TypeGenericAlias` which is what these types are.

Everything else looks good to me!


---

_Label `ty` added by @dhruvmanila on 2025-11-14 08:25_

---

_Renamed from "Add support for `P.args` and `P.kwargs`" to "[ty] Add support for `P.args` and `P.kwargs`" by @dhruvmanila on 2025-11-14 08:25_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 08:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-05 16:25:18.961900308 +0000
+++ new-output.txt	2025-12-05 16:25:22.658918282 +0000
@@ -3,22 +3,20 @@
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
-aliases_explicit.py:60:5: error[type-assertion-failure] Type `(...) -> None` does not match asserted type `@Todo(Callable[..] specialized with ParamSpec)`
 aliases_explicit.py:67:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 aliases_explicit.py:68:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 aliases_explicit.py:69:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:70:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:71:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_explicit.py:102:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
-aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
-aliases_implicit.py:72:5: error[type-assertion-failure] Type `(...) -> None` does not match asserted type `@Todo(Callable[..] specialized with ParamSpec)`
 aliases_implicit.py:76:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 aliases_implicit.py:77:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 aliases_implicit.py:78:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:79:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:80:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
 aliases_implicit.py:81:25: error[invalid-type-arguments] Type `str` is not assignable to upper bound `int | float` of type variable `TFloat@GoodTypeAlias12`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
@@ -44,7 +42,7 @@
 aliases_newtype.py:61:38: error[invalid-newtype] invalid base for `typing.NewType`: type `TD1`
 aliases_newtype.py:63:15: error[invalid-newtype] Wrong number of arguments in `NewType` creation, expected 2, found 3
 aliases_newtype.py:65:38: error[invalid-newtype] invalid base for `typing.NewType`: type `Any`
-aliases_type_statement.py:10:35: error[invalid-type-arguments] Too many type arguments: expected 1, got 3
+aliases_type_statement.py:10:52: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
 aliases_type_statement.py:17:1: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `bit_count`
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
 aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
@@ -65,14 +63,8 @@
 aliases_type_statement.py:47:23: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 aliases_type_statement.py:48:23: error[invalid-type-form] Boolean operations are not allowed in type expressions
 aliases_type_statement.py:49:23: error[fstring-type-annotation] Type expressions cannot use f-strings
-aliases_type_statement.py:75:107: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
 aliases_type_statement.py:77:27: error[invalid-type-arguments] Type `str` is not assignable to upper bound `int` of type variable `S@RecursiveTypeAlias2`
-aliases_type_statement.py:77:37: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
-aliases_type_statement.py:78:37: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
 aliases_type_statement.py:79:32: error[invalid-type-arguments] Type `int` is not assignable to upper bound `str` of type variable `T@RecursiveTypeAlias2`
-aliases_type_statement.py:79:37: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
-aliases_type_statement.py:80:37: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
-aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_type_statement.py:82:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias3`
 aliases_type_statement.py:88:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias6`
 aliases_type_statement.py:89:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias7`
@@ -158,7 +150,7 @@
 callables_protocol.py:169:7: error[invalid-assignment] Object of type `def cb8_bad1(x: int) -> Any` is not assignable to `Proto8`
 callables_protocol.py:186:5: error[invalid-assignment] Object of type `Literal["str"]` is not assignable to attribute `other_attribute` of type `int`
 callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`.
-callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[Unknown, Unknown]` has no attribute `other_attribute2`
+callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[(x: int), Unknown] | Proto9[Divergent, Unknown]` has no attribute `other_attribute2`
 callables_protocol.py:238:8: error[invalid-assignment] Object of type `def cb11_bad1(x: int, y: str, /) -> Any` is not assignable to `Proto11`
 callables_protocol.py:260:8: error[invalid-assignment] Object of type `def cb12_bad1(*args: Any, *, kwarg0: Any) -> None` is not assignable to `Proto12`
 callables_protocol.py:284:27: error[invalid-assignment] Object of type `def cb13_no_default(path: str) -> str` is not assignable to `Proto13_Default`
@@ -244,29 +236,41 @@
 constructors_call_type.py:59:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
 constructors_call_type.py:81:5: error[missing-argument] No argument provided for required parameter `y` of function `__new__`
 constructors_call_type.py:82:12: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `Literal[2]`
-constructors_callable.py:36:13: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:36:13: info[revealed-type] Revealed type: `(x: int) -> Unknown`
 constructors_callable.py:37:1: error[type-assertion-failure] Type `Class1` does not match asserted type `Unknown`
-constructors_callable.py:49:13: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:38:1: error[missing-argument] No argument provided for required parameter `x`
+constructors_callable.py:39:1: error[missing-argument] No argument provided for required parameter `x`
+constructors_callable.py:39:4: error[unknown-argument] Argument `y` does not match any known parameter
+constructors_callable.py:49:13: info[revealed-type] Revealed type: `() -> Unknown`
 constructors_callable.py:50:1: error[type-assertion-failure] Type `Class2` does not match asserted type `Unknown`
+constructors_callable.py:51:4: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:64:1: error[type-assertion-failure] Type `Class3` does not match asserted type `Unknown`
 constructors_callable.py:73:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-constructors_callable.py:77:13: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:77:13: info[revealed-type] Revealed type: `(x: int) -> Unknown`
 constructors_callable.py:78:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
+constructors_callable.py:79:1: error[missing-argument] No argument provided for required parameter `x`
+constructors_callable.py:80:1: error[missing-argument] No argument provided for required parameter `x`
+constructors_callable.py:80:4: error[unknown-argument] Argument `y` does not match any known parameter
 constructors_callable.py:97:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:100:5: error[type-assertion-failure] Type `Never` does not match asserted type `Unknown`
 constructors_callable.py:105:5: error[type-assertion-failure] Type `Never` does not match asserted type `Unknown`
-constructors_callable.py:125:13: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:125:13: info[revealed-type] Revealed type: `() -> Unknown`
 constructors_callable.py:126:1: error[type-assertion-failure] Type `Class6Proxy` does not match asserted type `Unknown`
+constructors_callable.py:127:4: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 constructors_callable.py:142:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:162:5: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:162:5: info[revealed-type] Revealed type: `Overload[(x: int) -> Unknown, (x: str) -> Unknown]`
 constructors_callable.py:164:1: error[type-assertion-failure] Type `Class7[int]` does not match asserted type `Unknown`
 constructors_callable.py:165:1: error[type-assertion-failure] Type `Class7[str]` does not match asserted type `Unknown`
-constructors_callable.py:182:13: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:182:13: info[revealed-type] Revealed type: `(x: list[Unknown], y: list[Unknown]) -> Unknown`
 constructors_callable.py:183:1: error[type-assertion-failure] Type `Class8[str]` does not match asserted type `Unknown`
-constructors_callable.py:193:13: info[revealed-type] Revealed type: `(...) -> Unknown`
+constructors_callable.py:193:13: info[revealed-type] Revealed type: `(x: list[T@__init__], y: list[T@__init__]) -> Unknown`
 constructors_callable.py:194:1: error[type-assertion-failure] Type `Class9` does not match asserted type `Unknown`
+constructors_callable.py:194:16: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
+constructors_callable.py:194:22: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
+constructors_callable.py:195:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | int]`
+constructors_callable.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
 dataclasses_descriptors.py:23:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | Desc1`
 dataclasses_descriptors.py:50:63: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[T@Desc2] | T@Desc2`
 dataclasses_descriptors.py:66:1: error[type-assertion-failure] Type `int` does not match asserted type `int | Desc2[int]`
@@ -403,7 +407,7 @@
 enums_member_values.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 enums_members.py:82:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x) -> Unknown)`
 enums_members.py:82:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:83:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((...) -> Unknown)`
+enums_members.py:83:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x: int) -> Unknown)`
 enums_members.py:83:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:84:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `property`
 enums_members.py:84:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -446,11 +450,7 @@
 generics_defaults.py:55:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:59:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:63:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:79:1: error[type-assertion-failure] Type `type[Class_ParamSpec[Unknown]]` does not match asserted type `<class 'Class_ParamSpec'>`
-generics_defaults.py:79:56: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
-generics_defaults.py:80:53: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
-generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
-generics_defaults.py:81:68: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
+generics_defaults.py:79:1: error[type-assertion-failure] Type `type[Class_ParamSpec[(str, int, /)]]` does not match asserted type `<class 'Class_ParamSpec'>`
 generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `<class 'Class_TypeVarTuple'>`
 generics_defaults.py:95:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `Class_TypeVarTuple`
 generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
@@ -468,32 +468,42 @@
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
 generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[@Todo(ParamSpecArgs / ParamSpecKwargs), ...]`
+generics_paramspec_components.py:17:25: error[invalid-type-form] `P.kwargs` is valid only in `**kwargs` annotation: Did you mean `P.args`?
+generics_paramspec_components.py:17:45: error[invalid-type-form] `P.args` is valid only in `*args` annotation: Did you mean `P.kwargs`?
+generics_paramspec_components.py:23:46: error[invalid-type-form] `P.args` is valid only in `*args` annotation: Did you mean `P.kwargs`?
+generics_paramspec_components.py:49:11: error[invalid-argument-type] Argument is incorrect: Expected `P@decorator.args`, found `P@decorator.kwargs`
+generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument is incorrect: Expected `P@decorator.kwargs`, found `P@decorator.args`
+generics_paramspec_components.py:51:11: error[invalid-argument-type] Argument is incorrect: Expected `P@decorator.args`, found `Literal[1]`
+generics_paramspec_components.py:83:18: error[invalid-argument-type] Argument to function `foo` is incorrect: Expected `int`, found `(...)`
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
-generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
+generics_paramspec_components.py:98:7: error[invalid-argument-type] Argument to function `twice` is incorrect: Expected `int`, found `Literal["A"]`
+generics_paramspec_components.py:98:20: error[invalid-argument-type] Argument to function `twice` is incorrect: Expected `str`, found `Literal[1]`
+generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(**P@changes_return_type_to_str) -> str`
 generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:22:1: error[type-assertion-failure] Type `(str, bool, /) -> str` does not match asserted type `(...) -> str`
-generics_paramspec_semantics.py:30:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
+generics_paramspec_semantics.py:26:4: error[positional-only-parameter-as-kwarg] Positional-only parameter 1 (`a`) passed as keyword argument
+generics_paramspec_semantics.py:26:11: error[positional-only-parameter-as-kwarg] Positional-only parameter 2 (`b`) passed as keyword argument
+generics_paramspec_semantics.py:27:9: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal["A"]`
+generics_paramspec_semantics.py:30:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(**P@func1) -> bool`
 generics_paramspec_semantics.py:34:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:38:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
+generics_paramspec_semantics.py:46:17: error[invalid-argument-type] Argument to function `func1` is incorrect: Expected `(x: int, y: str) -> int`, found `def y_x(y: int, x: str) -> int`
 generics_paramspec_semantics.py:53:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:57:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
+generics_paramspec_semantics.py:61:23: error[invalid-argument-type] Argument to function `func1` is incorrect: Expected `(*, x: int) -> int`, found `def keyword_only_y(*, y: int) -> int`
 generics_paramspec_semantics.py:76:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-generics_paramspec_semantics.py:82:28: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
-generics_paramspec_semantics.py:84:5: error[type-assertion-failure] Type `(int, /) -> str` does not match asserted type `(...) -> str`
 generics_paramspec_semantics.py:87:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:91:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
-generics_paramspec_semantics.py:101:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
+generics_paramspec_semantics.py:101:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(**P@remove) -> bool`
 generics_paramspec_semantics.py:113:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
 generics_paramspec_semantics.py:128:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:133:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:138:29: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:143:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_specialization.py:32:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, bool]`?
-generics_paramspec_specialization.py:40:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_paramspec_specialization.py:40:31: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_paramspec_specialization.py:52:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str, bool]`?
-generics_paramspec_specialization.py:58:27: error[invalid-type-arguments] Too many type arguments to class `ClassC`: expected 1, got 3
+generics_paramspec_specialization.py:44:27: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
+generics_paramspec_specialization.py:54:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal[""]`
+generics_paramspec_specialization.py:55:16: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal[""]`
+generics_paramspec_specialization.py:60:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal[""]`
+generics_paramspec_specialization.py:61:16: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal[""]`
 generics_scoping.py:14:1: error[type-assertion-failure] Type `int` does not match asserted type `Literal[1]`
 generics_scoping.py:15:1: error[type-assertion-failure] Type `str` does not match asserted type `Literal["a"]`
 generics_scoping.py:42:1: error[type-assertion-failure] Type `str` does not match asserted type `Literal["abc"]`
@@ -575,9 +585,9 @@
 generics_syntax_infer_variance.py:165:45: error[invalid-assignment] Object of type `ShouldBeContravariant1[int]` is not assignable to `ShouldBeContravariant1[int | float]`
 generics_syntax_infer_variance.py:166:43: error[invalid-assignment] Object of type `ShouldBeContravariant1[int | float]` is not assignable to `ShouldBeContravariant1[int]`
 generics_syntax_scoping.py:35:7: error[unresolved-reference] Name `T` used when not defined
-generics_syntax_scoping.py:40:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R@decorator1, /) -> (...) -> R@decorator1`
+generics_syntax_scoping.py:40:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((**P@decorator1) -> R@decorator1, /) -> (**P@decorator1) -> R@decorator1`
 generics_syntax_scoping.py:44:17: error[unresolved-reference] Name `T` used when not defined
-generics_syntax_scoping.py:81:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R@decorator2, /) -> (...) -> R@decorator2`
+generics_syntax_scoping.py:81:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((**P@decorator2) -> R@decorator2, /) -> (**P@decorator2) -> R@decorator2`
 generics_syntax_scoping.py:113:9: error[type-assertion-failure] Type `str` does not match asserted type `Literal[""]`
 generics_syntax_scoping.py:116:13: error[type-assertion-failure] Type `TypeVar` does not match asserted type `typing.TypeVar`
 generics_syntax_scoping.py:121:9: error[type-assertion-failure] Type `int | float | complex` does not match asserted type `complex`
@@ -1021,4 +1031,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1023 diagnostics
+Found 1033 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-14 08:28_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:2565:13: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `Coroutine[Any, Any, _T@run_coroutine_threadsafe]`, found `CoroutineType[Any, Any, T_Retval@run_async_from_thread]`
- src/anyio/functools.py:199:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(...) -> Awaitable[Unknown]`, found `((...) -> Coroutine[Any, Any, T@_LRUCacheWrapper]) | ((...) -> T@_LRUCacheWrapper)`
+ src/anyio/functools.py:199:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(...) -> Awaitable[Unknown]`, found `((**P@__call__) -> Coroutine[Any, Any, T@_LRUCacheWrapper]) | ((...) -> T@_LRUCacheWrapper)`
- Found 89 diagnostics
+ Found 90 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:351:9: error[invalid-method-override] Invalid override of method `inner`: Definition is incompatible with `_KeywordDecorator.inner`
- Found 167 diagnostics
+ Found 168 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/corofunc_cache.py:46:51: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:46:73: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:50:48: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:50:58: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `CoroFunc[Unknown]`
+ src/async_utils/corofunc_cache.py:50:58: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `CoroFunc[P@f__call__, R@f__call__]`
+ src/async_utils/corofunc_cache.py:148:12: error[invalid-return-type] Return type does not match returned value: expected `CoroCacheDeco`, found `def wrapper[**P, R](coro: CoroLike[P@wrapper, R@wrapper], /) -> CoroFunc[P@wrapper, R@wrapper]`
+ src/async_utils/corofunc_cache.py:231:12: error[invalid-return-type] Return type does not match returned value: expected `CoroCacheDeco`, found `def wrapper[**P, R](coro: CoroLike[P@wrapper, R@wrapper], /) -> CoroFunc[P@wrapper, R@wrapper]`
+ src/async_utils/gen_transform.py:110:36: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `Queue[Y@_consumer]`, found `Queue[Y@_sync_to_async_gen]`
+ src/async_utils/gen_transform.py:110:48: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `(**P@_consumer) -> Generator[Y@_consumer, None, None]`, found `(**P@_sync_to_async_gen) -> Generator[Y@_sync_to_async_gen, None, None]`
+ src/async_utils/gen_transform.py:110:60: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `P@_consumer.args`, found `P@_sync_to_async_gen.args`
+ src/async_utils/gen_transform.py:110:63: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `P@_consumer.kwargs`, found `P@_sync_to_async_gen.kwargs`
- src/async_utils/corofunc_cache.py:50:70: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:116:43: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:116:65: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:199:43: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/corofunc_cache.py:199:65: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/gen_transform.py:62:64: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/gen_transform.py:108:24: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:32:41: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:32:58: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:48:38: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:49:26: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:53:52: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:53:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `TaskFunc[Unknown]`
+ src/async_utils/task_cache.py:53:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `TaskFunc[P@f__call__, R@f__call__]`
+ src/async_utils/task_cache.py:213:12: error[invalid-return-type] Return type does not match returned value: expected `TaskCacheDeco`, found `def wrapper[**P, R](coro: TaskCoroFunc[P@wrapper, R@wrapper], /) -> TaskFunc[P@wrapper, R@wrapper]`
+ src/async_utils/task_cache.py:301:12: error[invalid-return-type] Return type does not match returned value: expected `TaskCacheDeco`, found `def wrapper[**P, R](coro: TaskCoroFunc[P@wrapper, R@wrapper], /) -> TaskFunc[P@wrapper, R@wrapper]`
- src/async_utils/task_cache.py:53:74: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:85:43: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:85:62: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:181:47: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:181:69: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:269:47: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- src/async_utils/task_cache.py:269:69: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
- Found 29 diagnostics
+ Found 15 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/jinja2/async_utils.py:41:9: error[unresolved-attribute] Unresolved attribute `jinja_async_variant` on type `_Wrapped[Unknown, Unknown, Unknown, Unknown]`.
+ lib/spack/spack/vendor/jinja2/async_utils.py:41:9: error[unresolved-attribute] Unresolved attribute `jinja_async_variant` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
- lib/spack/spack/vendor/jinja2/compiler.py:56:12: error[invalid-return-type] Return type does not match returned value: expected `F@optimizeconst`, found `_Wrapped[Unknown, Unknown, Unknown, Unknown]`
+ lib/spack/spack/vendor/jinja2/compiler.py:56:12: error[invalid-return-type] Return type does not match returned value: expected `F@optimizeconst`, found `_Wrapped[(...), Unknown, (...), Unknown]`

beartype (https://github.com/beartype/beartype)
- beartype/bite/kind/infercallable.py:562:9: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
+ beartype/bite/kind/infercallable.py:562:18: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `__name__`
+ asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `__name__`
- asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `__name__`
+ asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `__name__`
- asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, Unknown].asynq(...) -> AsyncTask[Any]`.
+ asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, ()].asynq(...) -> AsyncTask[Any]`.
+ asynq/tests/test_tools.py:72:39: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:74:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:82:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilterfalse]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:90:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asift]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:95:45: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | None]`
+ asynq/tests/test_tools.py:96:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:98:58: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | None | str]`
+ asynq/tests/test_tools.py:103:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | None]`
+ asynq/tests/test_tools.py:104:37: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | bool | None]`
+ asynq/tests/test_tools.py:105:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:109:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `Literal[1]`
+ asynq/tests/test_tools.py:109:26: error[too-many-positional-arguments] Too many positional arguments to bound method `__call__`: expected 2, got 3
+ asynq/tests/test_tools.py:110:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int | None]`
+ asynq/tests/test_tools.py:111:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `GeneratorType[Literal[1] | None, None, None]`
+ asynq/tests/test_tools.py:112:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:112:36: error[too-many-positional-arguments] Too many positional arguments to bound method `__call__`: expected 2, got 4
+ asynq/tests/test_tools.py:113:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | list[Unknown | int]]`
+ asynq/tests/test_tools.py:115:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `Literal[1]`
+ asynq/tests/test_tools.py:115:26: error[too-many-positional-arguments] Too many positional arguments to bound method `__call__`: expected 2, got 5
+ asynq/tests/test_tools.py:118:9: error[missing-argument] No argument provided for required parameter `arg` of bound method `__call__`
+ asynq/tests/test_tools.py:122:33: error[unknown-argument] Argument `random_keyword_argument` does not match any known parameter of bound method `__call__`
+ asynq/tests/test_tools.py:126:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `Literal[1]`
+ asynq/tests/test_tools.py:126:35: error[parameter-already-assigned] Multiple values provided for parameter `key` of bound method `__call__`
+ asynq/tests/test_tools.py:127:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | int | None]`
+ asynq/tests/test_tools.py:128:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `GeneratorType[Literal[1] | None, None, None]`
+ asynq/tests/test_tools.py:129:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:129:30: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amin, /) -> Any) | None`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:129:41: error[too-many-positional-arguments] Too many positional arguments to bound method `__call__`: expected 3, got 4
+ asynq/tests/test_tools.py:129:49: error[parameter-already-assigned] Multiple values provided for parameter `key` of bound method `__call__`
+ asynq/tests/test_tools.py:130:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | list[Unknown | int]]`
+ asynq/tests/test_tools.py:132:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `Literal[1]`
+ asynq/tests/test_tools.py:132:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amin, /) -> Any) | None`, found `Literal[2]`
+ asynq/tests/test_tools.py:132:29: error[too-many-positional-arguments] Too many positional arguments to bound method `__call__`: expected 3, got 5
+ asynq/tests/test_tools.py:135:9: error[missing-argument] No argument provided for required parameter `__arg` of bound method `__call__`
+ asynq/tests/test_tools.py:139:33: error[unknown-argument] Argument `random_keyword_argument` does not match any known parameter of bound method `__call__`
- asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `dirty`
+ asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `dirty`
- asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `dirty`
+ asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `dirty`
+ asynq/tests/test_typing.py:41:43: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `_T@generic`, found `str`
- asynq/tests/test_typing.py:44:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- asynq/tests/test_typing.py:45:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- asynq/tests/test_typing.py:46:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ asynq/tests/test_typing.py:54:16: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((...) -> _T@async_call) | ((...) -> FutureBase[_T@async_call])`, found `def f(x: int) -> str`
+ asynq/tests/test_typing.py:56:20: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((...) -> _T@async_call) | ((...) -> FutureBase[_T@async_call])`, found `def f(x: int) -> str`
+ asynq/tests/test_typing.py:61:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `Literal[1]`
+ asynq/tests/test_typing.py:61:29: error[too-many-positional-arguments] Too many positional arguments to bound method `__call__`: expected 2, got 3
+ asynq/tests/test_typing.py:61:32: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amax, /) -> Any) | None`, found `def len(obj: Sized, /) -> int`
+ asynq/tests/test_typing.py:62:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int]`
+ asynq/tests/test_typing.py:62:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amax, /) -> Any) | None`, found `def len(obj: Sized, /) -> int`
+ asynq/tests/typing_example/param_spec.py:15:21: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `int`, found `Literal["1"]`
+ asynq/tests/typing_example/param_spec.py:15:31: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `str`, found `Literal[2]`
- asynq/tools.py:206:9: error[unresolved-attribute] Unresolved attribute `__acached_per_instance_cache__` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:206:9: error[unresolved-attribute] Unresolved attribute `__acached_per_instance_cache__` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:268:17: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `alazy_constant_refresh_time`
+ asynq/tools.py:268:17: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `alazy_constant_refresh_time`
- asynq/tools.py:269:33: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `alazy_constant_refresh_time`
+ asynq/tools.py:269:33: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `alazy_constant_refresh_time`
- asynq/tools.py:271:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:271:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:272:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:272:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:273:20: error[unresolved-attribute] Object of type `AsyncDecorator[Any, Unknown]` has no attribute `alazy_constant_cached_value`
+ asynq/tools.py:273:20: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `alazy_constant_cached_value`
- asynq/tools.py:276:13: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:276:13: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:278:9: error[unresolved-attribute] Unresolved attribute `dirty` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:278:9: error[unresolved-attribute] Unresolved attribute `dirty` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:279:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:279:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:280:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:280:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, (...)]`.
- asynq/tools.py:310:9: error[unresolved-attribute] Unresolved attribute `original_fn` on type `AsyncDecorator[Any, Unknown]`.
+ asynq/tools.py:310:9: error[unresolved-attribute] Unresolved attribute `original_fn` on type `AsyncDecorator[Any, (...)]`.
- Found 176 diagnostics
+ Found 218 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:216:59: error[invalid-assignment] Object of type `staticmethod[Unknown, Unknown]` is not assignable to `(Any, /) -> Unknown`
+ src/graphql/execution/execute.py:216:59: error[invalid-assignment] Object of type `staticmethod[(value: Any), Unknown]` is not assignable to `(Any, /) -> Unknown`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/error.py:111:26: error[unresolved-attribute] Object of type `(...) -> R@checked_call` has no attribute `__name__`
+ src/_pytest/_py/error.py:111:26: error[unresolved-attribute] Object of type `(**P@checked_call) -> R@checked_call` has no attribute `__name__`
+ src/_pytest/_py/path.py:760:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
+ src/_pytest/_py/path.py:763:55: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
+ src/_pytest/_py/path.py:1271:53: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(suffix: str | None = None, prefix: str | None = None, dir: str | PathLike[str] | None = None) -> Unknown, (suffix: bytes | None = None, prefix: bytes | None = None, dir: bytes | PathLike[bytes] | None = None) -> Unknown]`, found `str`
+ src/_pytest/unittest.py:583:14: error[missing-argument] No argument provided for required parameter `cls`
+ src/_pytest/unittest.py:583:14: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_config.py:1426:14: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_config.py:1426:14: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_config.py:1748:10: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_config.py:1748:10: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_faulthandler.py:216:10: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_faulthandler.py:216:10: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_monkeypatch.py:426:10: error[missing-argument] No argument provided for required parameter `cls`
+ testing/test_monkeypatch.py:426:10: error[missing-argument] No argument provided for required parameter `cls`
- Found 433 diagnostics
+ Found 446 diagnostics

rich (https://github.com/Textualize/rich)
- examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def beat(length: int = Literal[1]) -> None`
+ examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(length: int = Literal[1]) -> Iterator[Unknown]`, found `def beat(length: int = Literal[1]) -> None`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/decorators.py:31:54: error[unresolved-attribute] Object of type `(...) -> _T@deprecated` has no attribute `__name__`
+ scrapy/utils/decorators.py:31:54: error[unresolved-attribute] Object of type `(**_P@deprecated) -> _T@deprecated` has no attribute `__name__`
- scrapy/utils/decorators.py:98:51: error[unresolved-attribute] Object of type `(...) -> _T@_warn_spider_arg` has no attribute `__qualname__`
+ scrapy/utils/decorators.py:98:51: error[unresolved-attribute] Object of type `(**_P@_warn_spider_arg) -> _T@_warn_spider_arg` has no attribute `__qualname__`

starlette (https://github.com/encode/starlette)
- starlette/applications.py:86:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ServerErrorMiddleware'>`
+ starlette/applications.py:86:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ServerErrorMiddleware'>`
- starlette/applications.py:88:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ExceptionMiddleware'>`
+ starlette/applications.py:88:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ExceptionMiddleware'>`
- starlette/applications.py:241:33: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ starlette/applications.py:241:33: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:84:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_base.py:84:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CustomMiddleware'>`
- tests/middleware/test_base.py:152:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'aMiddleware'>`
+ tests/middleware/test_base.py:152:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'aMiddleware'>`
- tests/middleware/test_base.py:153:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'bMiddleware'>`
+ tests/middleware/test_base.py:153:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'bMiddleware'>`
- tests/middleware/test_base.py:154:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'cMiddleware'>`
+ tests/middleware/test_base.py:154:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'cMiddleware'>`
- tests/middleware/test_base.py:169:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_base.py:169:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CustomMiddleware'>`
- tests/middleware/test_base.py:187:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_base.py:187:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CustomMiddleware'>`
- tests/middleware/test_base.py:277:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:277:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:315:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:315:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:335:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:335:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:362:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:362:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:433:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:433:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:434:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ContextManagerMiddleware'>`
+ tests/middleware/test_base.py:434:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ContextManagerMiddleware'>`
- tests/middleware/test_base.py:609:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:609:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:638:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:638:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:667:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:667:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:693:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:693:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:725:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:725:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:754:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:754:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:843:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:843:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:871:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'ConsumingMiddleware'>`
+ tests/middleware/test_base.py:871:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'ConsumingMiddleware'>`
- tests/middleware/test_base.py:1086:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'MyMiddleware'>`
+ tests/middleware/test_base.py:1086:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'MyMiddleware'>`
- tests/middleware/test_base.py:1220:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'BaseHTTPMiddleware'>`
+ tests/middleware/test_base.py:1220:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'BaseHTTPMiddleware'>`
- tests/middleware/test_base.py:1278:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'PassthroughMiddleware'>`
+ tests/middleware/test_base.py:1278:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'PassthroughMiddleware'>`
- tests/middleware/test_cors.py:20:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:20:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:81:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:81:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:132:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:132:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:181:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:181:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:222:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:222:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:255:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:255:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:285:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:285:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:310:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:310:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:382:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:382:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:415:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:415:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:436:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:436:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:456:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:456:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:473:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:473:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:492:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:492:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:513:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:513:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:543:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:543:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_cors.py:583:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
+ tests/middleware/test_cors.py:583:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CORSMiddleware'>`
- tests/middleware/test_gzip.py:23:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:23:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:41:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:41:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:61:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:61:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:84:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:84:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:107:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:107:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:132:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:132:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:155:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:155:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_gzip.py:180:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'GZipMiddleware'>`
+ tests/middleware/test_gzip.py:180:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'GZipMiddleware'>`
- tests/middleware/test_https_redirect.py:16:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'HTTPSRedirectMiddleware'>`
+ tests/middleware/test_https_redirect.py:16:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'HTTPSRedirectMiddleware'>`
- tests/middleware/test_middleware.py:16:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_middleware.py:16:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CustomMiddleware'>`
- tests/middleware/test_middleware.py:21:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'CustomMiddleware'>`
+ tests/middleware/test_middleware.py:21:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'CustomMiddleware'>`
- tests/middleware/test_session.py:35:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `<class 'SessionMiddleware'>`
+ tests/middleware/test_session.py:35:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'SessionMiddleware'>`
- tests/middleware/test_session.py:67:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: E

... (truncated 1638 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~3MB
+     struct fields = ~4MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~52MB
+     struct fields = ~54MB


```

</details>




---

_Comment by @dhruvmanila on 2025-11-20 06:43_

There's a stack overflow in `mypy_primer` repository which I'm trying to figure out why it's happening on this branch, I've narrowed it down to the following code:

```py
from dataclasses import field

field(default_factory=dict)
```

---

_Comment by @dhruvmanila on 2025-11-20 07:10_

Hmm, not really sure where is the stack overflow being triggered, a more minimal code that behaves the same way as above but without the `dataclasses` import:

```py
from typing import Callable


def f[_T](*, default_factory: Callable[[], _T]) -> _T:
    return default_factory()


f(default_factory=dict)
```

It's important that the `default_factory` is `dict` and not any other type like `list`, `int`, etc.

---

_Comment by @dhruvmanila on 2025-11-20 07:20_

Nevermind, found the issue, it's a silly mistake on my end.

---

_@dhruvmanila reviewed on 2025-12-02 06:21_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/decorators.md`:131 on 2025-12-02 06:21_

I'm looking into this, a minimal repro:

```py
from typing import Callable


class _Wrapped[**P, R]:
    def __call__(self, *args: P.args, **kwargs: P.kwargs) -> R: ...


def update_wrapper[**P, R](wrapper: Callable[P, R]) -> _Wrapped[P, R]: ...


def wrapper(*args, **kwargs): ...


# bound method _Wrapped[(...), Unknown].__call__(**P@_Wrapped) -> Unknown
#
# This should actually reveal `(...) -> Unknown`
reveal_type(update_wrapper(wrapper).__call__)
```

---

_Comment by @codspeed-hq[bot] on 2025-12-02 09:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fparamspec-args-kwargs?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21445 will **not alter performance**

<sub>Comparing <code>dhruv/paramspec-args-kwargs</code> (e5a4f59) with <code>main</code> (f29436c)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fparamspec-args-kwargs?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@dhruvmanila reviewed on 2025-12-02 13:16_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/decorators.md`:131 on 2025-12-02 13:16_

Oh man, this was hard to track! I was not passing around the ParamSpec type variable correctly in all places. Fixed this in [`b358231` (#21445)](https://github.com/astral-sh/ruff/pull/21445/commits/b358231686abed80e64c6c85dd68433a9a3f0693).

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-12-03 12:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 12:32_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 209 | 0 | 96 |
| `invalid-type-arguments` | 5 | 151 | 0 |
| `no-matching-overload` | 147 | 0 | 0 |
| `unresolved-attribute` | 0 | 4 | 114 |
| `missing-argument` | 104 | 0 | 0 |
| `unused-ignore-comment` | 1 | 79 | 0 |
| `invalid-type-form` | 0 | 45 | 1 |
| `unknown-argument` | 43 | 2 | 0 |
| `invalid-assignment` | 7 | 0 | 21 |
| `invalid-return-type` | 1 | 3 | 10 |
| `too-many-positional-arguments` | 10 | 0 | 0 |
| `possibly-missing-attribute` | 0 | 0 | 7 |
| `invalid-method-override` | 4 | 0 | 0 |
| `invalid-parameter-default` | 0 | 0 | 3 |
| `parameter-already-assigned` | 2 | 0 | 0 |
| `unsupported-base` | 1 | 1 | 0 |
| `invalid-await` | 0 | 0 | 1 |
| `redundant-cast` | 0 | 1 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **535** | **286** | **253** |

**[Full report with detailed diff](https://dhruv-paramspec-args-kwargs.ecosystem-663.pages.dev/diff)** ([timing results](https://dhruv-paramspec-args-kwargs.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @dhruvmanila on 2025-12-03 17:57_

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-12-03 17:57_

---

_Renamed from "[ty] Add support for `P.args` and `P.kwargs`" to "[ty] Complete support for `ParamSpec`" by @dhruvmanila on 2025-12-03 18:18_

---

_Marked ready for review by @dhruvmanila on 2025-12-03 18:22_

---

_Review requested from @carljm by @dhruvmanila on 2025-12-03 18:22_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-12-03 18:22_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-03 18:22_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-03 18:22_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-12-03 18:22_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/hover.rs`:2295 on 2025-12-04 16:36_

shouldn't this be contravariant? Is that worth a TODO comment?

E.g. when we hover over `T` here, we display it as being contravariant:

```py
from typing import Callable
type X[T] = Callable[[T], int]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:68 on 2025-12-04 16:40_

ideally I think we'd emit a diagnostic here -- an arbitrary instance of `ParamSpec` should not be valid in a type expression. (You could add a TODO comment for now.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:121 on 2025-12-04 16:41_

or as a type parameter to `Protocol`/`Generic`. Maybe:

```suggestion
In type annotations, `ParamSpec` is only valid as the first element to `Callable` or the final element to `Concatenate`.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:188 on 2025-12-04 16:44_

it could be worth having one test where the variadic parameters have different names, just to demonstrate that it's the _kind_ of the parameter that matters rather than the name. E.g. this is fine:

```py
class Foo(Generic[P]):
    def __call__(self, *arguments: P.args, **keyword_arguments: P.kwargs) -> None: ...
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:248 on 2025-12-04 16:50_

is this not one of the things we should emit an `[invalid-type-arguments]` diagnostic on? `Any` is not a ParamSpec, `...` or a list of types

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:257 on 2025-12-04 16:51_

```suggestion
Nor can they be omitted when there are more than one `ParamSpec`s.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:373 on 2025-12-04 16:55_

it could also be worth adding a test where a class is generic over `PAnother` but _not_ generic over `P`. I think that should cause us to omit a diagnostic, since `PAnother` would have `P` as its default, but `P` wouldn't be in scope?

(It's obviously fine to just add a TODO to that test if it's not implemented yet)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:181 on 2025-12-04 17:02_

I don't think this TODO is accurate, because `None` is a subtype of `object`: `object | None` just immediately simplifies to `object`. The existing assertion is correct!

```suggestion
    reveal_type(kwargs.get("key"))  # revealed: object
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:235 on 2025-12-04 17:03_

same as in the legacy markdown test, I think this is something we should emit an error on -- `Any` is not a list of types, a ParamSpec or `...`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:280 on 2025-12-04 17:04_

```suggestion
# `P1` hasn't been specialized, so it defaults to `...` gradual form
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:469 on 2025-12-04 17:09_

you could possibly also add an example of keyword-arguments with default values. E.g.

```py
def keyword_only_with_default_1(*, x: int = 42) -> int:
    return 1

def keyword_only_with_default_2(*, y: int = 42) -> int:
    return 2
```

In this case, the behavioural supertype of the parameter sets is `()`, since both `keyword_only_with_default_1` and `keyword_only_with_default_2` can be called with 0 arguments passed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:511 on 2025-12-04 17:13_

and also add a call to `foo1_with_extra_arg` that _doesn't_ cause us to emit a diagnostic?

```suggestion
    # error: [invalid-argument-type] "Argument to function `foo1_with_extra_arg` is incorrect: Expected `str`, found `P2@foo2.args`"
    foo1_with_extra_arg(func, *args, **kwargs)
    
    foo1_with_extra_arg(func, "foooooo", *args, **kwargs)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:566 on 2025-12-04 17:17_

I think the "union with `Unknown`" thing is making this test a bit more confusing than it needs to be? Could you just explicitly annotate the instance attributes, so that they aren't unioned with `Unknown`?

````suggestion
```py
class Foo[**P]:
    def __init__(self, *args: P.args, **kwargs: P.kwargs) -> None:
        self.args: P.args = args
        self.kwargs: P.kwargs = kwargs

def bar[**P](foo: Foo[P]) -> None:
    reveal_type(foo)  # revealed: Foo[P@bar]
    reveal_type(foo.args)  # revealed: P@bar.args
    reveal_type(foo.kwargs)  # revealed: P@bar.kwargs
```

ty will check whether the argument after `**` is a mapping type:

```py
from typing import Callable

def baz[**P](fn: Callable[P, None], foo: Foo[P]) -> None:
    fn(*foo.args, **foo.kwargs)
    fn(*foo.kwargs, **foo.args)  # error
```
````

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/with/async.md`:216 on 2025-12-04 17:19_

I guess we're nearly there on this TODO now 😅

---

_@AlexWaygood reviewed on 2025-12-04 17:19_

Just a review of the tests so far, which are overall fantastic. So excited to see how powerful this makes some of our type inference!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4997 on 2025-12-04 17:47_

nit: I think you can do this without the `unreachable!()` call:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index c2d26b1124..a4871e093f 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -4985,14 +4985,17 @@ impl<'db> Type<'db> {
                     .into()
             }
 
-            Type::TypeVar(typevar)
-                if typevar.is_paramspec(db) && matches!(name_str, "args" | "kwargs") =>
-            {
-                Place::declared(Type::TypeVar(match name_str {
-                    "args" => typevar.with_paramspec_attr(db, ParamSpecAttrKind::Args),
-                    "kwargs" => typevar.with_paramspec_attr(db, ParamSpecAttrKind::Kwargs),
-                    _ => unreachable!(),
-                }))
+            Type::TypeVar(typevar) if name_str == "args" && typevar.is_paramspec(db) => {
+                Place::declared(Type::TypeVar(
+                    typevar.with_paramspec_attr(db, ParamSpecAttrKind::Args),
+                ))
+                .into()
+            }
+
+            Type::TypeVar(typevar) if name_str == "kwargs" && typevar.is_paramspec(db) => {
+                Place::declared(Type::TypeVar(
+                    typevar.with_paramspec_attr(db, ParamSpecAttrKind::Kwargs),
+                ))
                 .into()
             }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8609 on 2025-12-04 17:52_

This TODO comment looks a bit orphaned -- what's it meant to refer to?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9718 on 2025-12-04 17:56_

I think it would be better here to do an exhaustive match over `DynamicType` variants, because we have 3 `Todo` variants currently and we might add or remove some in the future:

```suggestion
                Type::Dynamic(dynamic) => match dynamic {
                    DynamicType::Todo(_) | DynamicType::TodoUnpack | DynamicType::TodoStarredExpression => Parameters::todo(),
                    DynamicType::Any | DynamicType::Unknown | DynamicType::Divergent(_) | DynamicType::UnknownDivergent(_) => Parameters::unknown(),
                }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9751 on 2025-12-04 18:02_

(optional nit)

```suggestion
                if known_class == Some(KnownClass::ParamSpec) {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9920 on 2025-12-04 18:06_

for most other Salsa-interned types, we have this note about ordering:

```suggestion
/// A type variable that has been bound to a generic context, and which can be specialized to a
/// concrete type.
///
/// # Ordering
///
/// Ordering is based on the wrapped data's salsa-assigned id and not on its values.
/// The id may change between runs, or when e.g. a `BoundTypeVarInstance` was garbage-collected and recreated.
#[salsa::interned(debug, heap_size=ruff_memory_usage::heap_size)]
#[derive(PartialOrd, Ord)]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9909 on 2025-12-04 18:09_

nit: add some docs?

```suggestion
    /// If `Some()`, this indicates that this type variable is the `.args` or `.kwargs`
    /// attribute of a `ParamSpec`
    paramspec_attr: Option<ParamSpecAttrKind>,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9926 on 2025-12-04 18:10_

```suggestion
    /// If `Some()`, this indicates that this type variable is the `.args` or `.kwargs`
    /// attribute of a `ParamSpec`
    paramspec_attr: Option<ParamSpecAttrKind>,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9927 on 2025-12-04 18:12_

nit: could improve the error message here if this assertion fails

```suggestion
        debug_assert!(
            self.is_paramspec(db),
            "Expected a paramspec but got a {:?}",
            self.kind(db)
        );
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9955 on 2025-12-04 18:12_

```suggestion
        debug_assert!(
            self.is_paramspec(db),
            "Expected a paramspec but got a {:?}",
            self.kind(db)
        );
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9941 on 2025-12-04 18:13_

```suggestion
            None, // `P.args` and `P.kwargs` cannot have defaults, even though `P` can
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9964 on 2025-12-04 18:13_

```suggestion
            None, // `P.args` and `P.kwargs` cannot have defaults, even though `P` can
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9940 on 2025-12-04 18:14_

```suggestion
            None, // ParamSpecs cannot have explicit variance
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9963 on 2025-12-04 18:14_

```suggestion
                None, // ParamSpecs cannot have explicit variance
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:2582 on 2025-12-04 18:25_

it will only be `Unknown | P.args` if the instance attribute was unannotated, as I mentioned in https://github.com/astral-sh/ruff/pull/21445#discussion_r2589923699

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:3169 on 2025-12-04 18:38_

(optional nit)

```suggestion
        if callable.kind(self.db) != CallableTypeKind::ParamSpecValue {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:1705 on 2025-12-04 18:42_

```suggestion
                write!(f, "**{}", typevar.name(self.db))?;
                if let Some(name) = typevar.binding_context(self.db).name(self.db) {
                    write!(f, "@{name}")?;
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1410 on 2025-12-04 18:43_

what's "this", in this context?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2639 on 2025-12-04 18:47_

I think what you have currently here is correct. We do know for a fact that `*args` will _always_ be a tuple of some kind.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2646 on 2025-12-04 18:47_

same here, I think `tuple[Unknown, ...]` (what you currently have) is the best answer here!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2766 on 2025-12-04 18:48_

and here, `dict[str, Unknown]` seems like the right answer to me; I'd just remove the TODO

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2782 on 2025-12-04 18:48_

same here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3430 on 2025-12-04 18:49_

does it _have_ to use `infer_type_expression`? We only get here in very specific situations; what you have here seems fine to me

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3485 on 2025-12-04 18:49_

```suggestion
                // This should be taken care of by the caller.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3491 on 2025-12-04 18:50_

nit: this assertion is probably very cheap; you could make this a non-debug assertion

```suggestion
                    assert!(
                        exactly_one_paramspec,
                        "Inferring ParamSpec value during explicit specialization for a \
                    tuple expression should only happen when it contains exactly one ParamSpec"
                    );
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3541 on 2025-12-04 18:51_

nit

```suggestion
                        // TODO: Emit diagnostic: "ParamSpec "P" is unbound"
```

---

_@AlexWaygood approved on 2025-12-04 18:56_

This looks fantastic!!

I'd appreciate it if @dcreager or @carljm could also take a look, as I don't consider myself an expert on our call-binding machinery or our generics machinery

---

_Review comment by @dcreager on `crates/ty_ide/src/hover.rs`:1 on 2025-12-04 21:07_

These edits are probably what's causing the `cargo fmt` CI failure. You'll need to revert the whitespace edits and only keep the "meaty" change below in the diff

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/with/async.md`:216 on 2025-12-04 21:09_

So close! I think #21551 is the rest of it!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:322 on 2025-12-04 21:14_

To clarify — as written, this function returns whether the generic context contains exactly 1 `ParamSpec` and exactly 0 other `TypeVar`s. The comment suggests you might instead have intended to return whether it contains 1 `ParamSpec` and _0 or more_ `TypeVar`s. If you meant the former I'd suggest rewording the docs:


```suggestion
    /// Returns `true` if this generic context contains exactly one `ParamSpec`
    /// and no other type variables.
```

If you meant the latter then the method body is wrong.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:204 on 2025-12-04 21:21_

We'll need this logic for `TypeMapping::PartialSpecialization` too. It might be worth pulling much of the body out into a helper method.

---

_@dcreager reviewed on 2025-12-04 21:24_

Some initial thoughts, still need to look through `call/bind.rs` in more detail

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:3131 on 2025-12-04 22:51_

For these helper methods, it would be nice to have some Python snippets in the doc comment describing the particular cases that they're meant to handle.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2729 on 2025-12-04 22:58_

Instead of repeating the match statement here, you might consider adding this logic as a method on `VariadicArgumentType`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:3371 on 2025-12-04 22:59_

Same as above — ideally we wouldn't have to repeat this match statement here, and you could add this as a method on `VariadicArgumentType`. Though there you don't have access to that type, so in this case it might not be worth the refactoring churn

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:3428 on 2025-12-04 23:02_

We have considered adding a new `Type` variant to handle heterogeneous lists. I don't know if there's an open issue for it. But if we ever do that, it will probably lean heavily on `TupleSpec`, so 👍 to doing it this way for now, since that will make it most likely that this logic would carry over easily.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:3544 on 2025-12-04 23:05_

🤮 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:1717 on 2025-12-04 23:13_

This is where the main conflict with #21551 will be. No suggested changes for this PR though!

To write down my plans for the merge conflict: #21551 adds a new `ConstraintSetAssignability` typing relation, and this logic will move over to `Type::has_relation_to_impl`, and will define what constraint set we create to describe when a callable lhs/actual is assignable to a `ParamSpec` callable rhs/formal.

---

_@dcreager reviewed on 2025-12-04 23:13_

This is looking very good!

---

_@dhruvmanila reviewed on 2025-12-05 04:13_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:68 on 2025-12-05 04:13_

This seems easy enough to do as I just need to remove the `ParamSpec` branch in `in_type_expression`, I'll do it in this PR.

---

_@dhruvmanila reviewed on 2025-12-05 04:50_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:248 on 2025-12-05 04:50_

Yes, I should've clarified that both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=baf1446b0bfbf475c1a3233c49d50276) and [Pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcAUCQMYA2AhgM61QBiYYA2gFTsAKANFACoBdAFxQAdBLIgApgDdp1SgH14CaQApmbInD6oYggJRA) supports specifying `Any` for a paramspec and I've also seen instances of this in the ecosystem report specifically used in `staticmethod[Any, Any]` or `classmethod[Any, Any]`.

This is similar to how `Concatenate` is allowed as well in that position, it's the `default` of a `ParamSpec` where the limitation of "ParamSpec, `...` or a list of types" exists as per the spec but I can't find any reference specifying what is allowed to pass as explicit specialization to a ParamSpec.

---

_@dhruvmanila reviewed on 2025-12-05 04:56_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:373 on 2025-12-05 04:56_

Yes, it should error because it's not in scope, I'll add the test case with a TODO as scoping rules aren't implemented in our generic implementation yet.

---

_@dhruvmanila reviewed on 2025-12-05 04:58_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:235 on 2025-12-05 04:58_

See https://github.com/astral-sh/ruff/pull/21445#discussion_r2591394627, happy to revert it if you think otherwise.

---

_@dhruvmanila reviewed on 2025-12-05 04:59_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:181 on 2025-12-05 04:59_

🤦 thanks! I was a little bit confused to see this but yes it's obvious now

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:280 on 2025-12-05 05:01_

I actually meant to say that it defaults to a gradual form where the type of parameters is `Unknown` aka `(*args: Unknown, **kwargs: Unknown)` but I guess that's more of an internal details. I'll use your suggestion.

---

_@dhruvmanila reviewed on 2025-12-05 05:01_

---

_@dhruvmanila reviewed on 2025-12-05 05:02_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:469 on 2025-12-05 05:02_

Yeah, that makes sense, thanks!

---

_@dhruvmanila reviewed on 2025-12-05 05:08_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:566 on 2025-12-05 05:08_

You cannot use `P.args` and `P.kwargs` in other position except for to annotate `*args` and `**kwargs` parameter, so `self.args: P.args = args` isn't allowed and will raise a diagnostic.

---

_@dhruvmanila reviewed on 2025-12-05 05:27_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:566 on 2025-12-05 05:27_

Hmm, actually, this prompted me to try something weird because this allows you to store the "args" and "kwargs" separately and then access it outside the class like so:

```py
from typing import Callable

class Foo[**P]:
    def __init__(self, fn: Callable[P, int], *args: P.args, **kwargs: P.kwargs) -> None:
        self.fn = fn
        self.args = args
        self.kwargs = kwargs

def fn(x: int, y: str) -> int:
    return 1

foo = Foo(fn, 1, "a")

reveal_type(foo)

# Pyright reveals here `Unknown`
# ty reveals `Unknown | ((x: int, y: str))`
# mypy has an INTERNAL ERROR here: https://mypy-play.net/?mypy=master&python=3.12&gist=55fbcad1f66e947123afe9920176fd32
reveal_type(foo.args)
reveal_type(foo.kwargs)

fn(*foo.args, **foo.kwargs)
fn(*foo.args)
fn(**foo.kwargs)
```

---

_@dhruvmanila reviewed on 2025-12-05 05:30_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2582 on 2025-12-05 05:30_

Right, but you cannot annotate a variable with `P.args` or `P.kwargs`

---

_@dhruvmanila reviewed on 2025-12-05 05:32_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:4997 on 2025-12-05 05:32_

Love this, thank you!

---

_@dhruvmanila reviewed on 2025-12-05 05:33_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:8609 on 2025-12-05 05:33_

Oops, yes this was from the first iteration of this PR. I'll remove this, thanks for catching it!

---

_@dhruvmanila reviewed on 2025-12-05 05:35_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:9718 on 2025-12-05 05:35_

Makes sense, thanks!

---

_@dhruvmanila reviewed on 2025-12-05 05:59_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/generics.rs`:1410 on 2025-12-05 05:59_

Consider the following example:

```py
from typing import Callable

def foo[**P](f1: Callable[P, int], f2: Callable[P, int]) -> Callable[P, bool]: ...

def f1(x: int) -> int:
    return 1

def f2(x: str) -> int:
    return ""

reveal_type(foo(f1, f2))
```

Here, `foo` is generic over a single `ParamSpec` which is used in multiple parameters. Later, when `P` is being specialized with two different parameter list, it will create a union which isn't then mapped in the return type. So, this allows us to pick the first specialization, type check the second occurrence using that and specialize the return type using that.

Without this early return, the reveal type would be `(**P@foo) -> bool`, right now it's `(x: int) -> bool`. This is mainly a workaround for now until ty supports specializing `P` here into a supertype considering all the parameter lists. I'll update the TODO and open a new follow-up issue to keep track.

---

_@dhruvmanila reviewed on 2025-12-05 06:03_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer/builder.rs`:3430 on 2025-12-05 06:03_

I don't think it has to use `infer_type_expression` but isn't `...` allowed in a few other places? I guess it's fine to special case this where required like here. I'll remove the TODO comment.

---

_@dhruvmanila reviewed on 2025-12-05 06:08_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer/builder.rs`:3491 on 2025-12-05 06:08_

I'm not sure if this provides any benefit as this would be a programming error faced by the user.

---

_@dhruvmanila reviewed on 2025-12-05 06:09_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/hover.rs`:1 on 2025-12-05 06:09_

Yup, my editor loves removing these whitespaces!

---

_@dhruvmanila reviewed on 2025-12-05 06:11_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/generics.rs`:322 on 2025-12-05 06:11_

Yes, I meant the former, I'll update the documentation.

---

_@dhruvmanila reviewed on 2025-12-05 06:33_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:204 on 2025-12-05 06:33_

Ah yes, I've extracted the common part as a nested function in this function body.

---

_@MichaReiser reviewed on 2025-12-05 07:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:3491 on 2025-12-05 07:11_

I have no opinion on this but a benefit of using `assert` is that we check the assertion in mypy primer.

---

_@dhruvmanila reviewed on 2025-12-05 07:17_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/hover.rs`:1 on 2025-12-05 07:17_

And the best PR of the year award goes to 🥁 https://github.com/astral-sh/ruff/pull/21806

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2729 on 2025-12-05 08:57_

I'm not sure how would that work. The `VariadicArgumentType` is specific to `*args` but this is handling the `**kwargs` (keyword variadic). Let me see if I can think of a way to de-duplicate this, currently it's used in `match_variadic_argument`, `match_keyword_variadic` and `check_keyword_variadic_argument_type` but they have slightly different semantics. I think the keyword variadic handling can possibly be clubbed together.

---

_@dhruvmanila reviewed on 2025-12-05 08:57_

---

_@AlexWaygood reviewed on 2025-12-05 13:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:566 on 2025-12-05 13:22_

Ahh, great point! I think I was getting mixed up with `TypeVarTuple`, where you of course _are_ allowed to annotate something inside the class as `self.x: tuple[*Ts]` if the class as a whole is generic over a `TypeVarTuple` `Ts`.

The mypy crash seems to have already been reported at https://github.com/python/mypy/issues/19839. We should definitely add tests (if you haven't already) for the (invalid) case where you attempt to annotate things inside the class with `self.x: P.args` and `self.y: P.kwargs`, so that we can make sure that we at least don't crash on that 😆

You might still be able to avoid the union-with-`Unknown` behaviour if you annotated `self.args` and `self.kwargs` as `Final`?

---

_@AlexWaygood reviewed on 2025-12-05 13:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3491 on 2025-12-05 13:23_

yeah, I have a general preference for cheap assertions to be always enabled where possible. It usually makes it easier to tell from the stack trace in bug reports where the bug is. And it can be confusing if an assertion triggers locally (using a debug build) but not on a released (non-debug) version of ty.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:248 on 2025-12-05 13:28_

Huh. I find that quite surprising. Could you add a comment saying that this doesn't seem to be explicitly permitted by the spec, but we match mypy's/pyright's behaviour here for compatibility? (And also the same comment for the equivalent test in the pep695 mdtest suite)

---

_@AlexWaygood reviewed on 2025-12-05 13:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:2582 on 2025-12-05 13:39_

but I can still cunningly force ty to infer a type of `P.args` (rather than `P.args | Unknown`) if I annotate the instance variable with `Final` on your branch:

```py
from typing import Final
from collections.abc import Callable

class F[**P]:
    def __init__(self, fn: Callable[P, int], *args: P.args, **kwargs: P.kwargs):
        self.args: Final = args
        self.kwargs: Final = kwargs

def f[**P](a: F[P]):
    reveal_type(a.args)  # revealed: P@f.args
    reveal_type(a.kwargs)  # revealed: P@f.kwargs
```

---

_@AlexWaygood reviewed on 2025-12-05 13:39_

---

_@dcreager reviewed on 2025-12-05 13:52_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2729 on 2025-12-05 13:52_

> I'm not sure how would that work. The `VariadicArgumentType` is specific to `*args` but this is handling the `**kwargs` (keyword variadic).

Ah I missed that distinction! Yeah you could theoretically have both `VariadicArgumentType` and `KeywordVariadicArgumentType`, which would possibly look very similar to each other.

In general I like avoiding repeating these kinds of tests since it adds cognitive load to keep them in sync. But I'm fine if you decide not to do this, or mark it as a TODO. It's not worth doing if it will be a heavy lift — or at least not in this PR.

---

_@dhruvmanila reviewed on 2025-12-05 14:42_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:248 on 2025-12-05 14:42_

Yeah, added a prose in both legacy and pep695 test case and in the actual source code as well.

---

_@dhruvmanila reviewed on 2025-12-05 14:49_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:566 on 2025-12-05 14:49_

> We should definitely add tests (if you haven't already) for the (invalid) case where you attempt to annotate things inside the class with `self.x: P.args` and `self.y: P.kwargs`, so that we can make sure that we at least don't crash on that 😆

For sure, I've added the test case, we don't error right now as I've planned it for follow-up.

> You might still be able to avoid the union-with-`Unknown` behaviour if you annotated `self.args` and `self.kwargs` as `Final`?

I think we should have both. The one without `Final` should be testing the special-case behavior that is implemented in `call/bind.rs` (`match_variadic`, `match_keyword_variadic` and `check_keyword_variadic_argument_type`).

---

_@AlexWaygood reviewed on 2025-12-05 14:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:566 on 2025-12-05 14:50_

That sounds great, thank you!!

---

_@dhruvmanila reviewed on 2025-12-05 15:04_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer/builder.rs`:3491 on 2025-12-05 15:04_

That all sounds reasonable, I've changed it to use `assert`

---

_@dhruvmanila reviewed on 2025-12-05 15:04_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:3131 on 2025-12-05 15:04_

Added doc comments with explanation and examples.

---

_@dhruvmanila reviewed on 2025-12-05 15:04_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2582 on 2025-12-05 15:04_

I've added this as a test case, thanks!

---

_@dhruvmanila reviewed on 2025-12-05 15:30_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2729 on 2025-12-05 15:30_

Makes sense, I'll try to take a stab at it before landing this PR but otherwise will prefer to do it as a follow-up.

---

_@dcreager approved on 2025-12-05 15:30_

❤️ I'm happy with where this is, I think we should go ahead and land this and address anything else as follow-on PRs. I can handle the merge conflict with #21551 

---

_@dhruvmanila reviewed on 2025-12-05 16:22_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/hover.rs`:2295 on 2025-12-05 16:22_

I'll add a TODO comment, I'm not actually sure about this but it does make sense for this to be Contravariant, the current variance is being inferred automatically.

---

_@dhruvmanila reviewed on 2025-12-05 16:23_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2729 on 2025-12-05 16:23_

I've noted this down, will do it as a follow-up instead.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/hover.rs`:2295 on 2025-12-05 16:25_

Yeah, I think it should either be contravariant or invariant... bivariant just seems definitely wrong, anyway 😆

---

_@AlexWaygood reviewed on 2025-12-05 16:25_

---

_@AlexWaygood reviewed on 2025-12-05 16:26_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/hover.rs`:2295 on 2025-12-05 16:26_

(A TODO comment sounds great 👍)

---

_Merged by @dhruvmanila on 2025-12-05 16:30_

---

_Closed by @dhruvmanila on 2025-12-05 16:30_

---

_Branch deleted on 2025-12-05 16:30_

---
