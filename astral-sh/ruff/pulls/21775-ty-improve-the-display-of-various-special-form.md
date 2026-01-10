```yaml
number: 21775
title: "[ty] Improve the display of various special-form types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/special-form-display
created_at: 2025-12-03T15:46:39Z
updated_at: 2025-12-03T21:20:00Z
url: https://github.com/astral-sh/ruff/pull/21775
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Improve the display of various special-form types

---

_Pull request opened by @AlexWaygood on 2025-12-03 15:46_

## Summary

The type _defined by_ `typing.Any`, and the type _inhabited by_ `typing.Any` are two very different types in our model. However, we currently display them almost exactly the same way! The former is displayed as `Any` in our diagnostics, and the latter is displayed as `typing.Any`, but we currently expect users to somehow psychically divine that these refer to two different types. This, understandably, causes confusion (see https://github.com/astral-sh/ty/issues/1744).

This PR improves our type display for the various strange, highly special-cased singleton types that we have in our model for tracking special forms in value positions. It should now be much more obvious that symbols inhabit very different types to the types they define.

## Test Plan

Mdtests


---

_Label `ty` added by @AlexWaygood on 2025-12-03 15:46_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-03 15:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 15:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-03 21:16:41.719454171 +0000
+++ new-output.txt	2025-12-03 21:16:45.404479261 +0000
@@ -502,16 +502,16 @@
 generics_scoping.py:75:11: error[invalid-generic-class] Generic class `Bad` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@prop1`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `list[typing.Self]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `typing.Self`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `list[<special form 'typing.Self'>]`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `<special form 'typing.Self'>`
 generics_self_advanced.py:38:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `Unknown`
 generics_self_advanced.py:42:9: error[type-assertion-failure] Type `type[Self@method3]` does not match asserted type `Unknown`
 generics_self_advanced.py:43:9: error[type-assertion-failure] Type `list[Self@method3]` does not match asserted type `Unknown`
 generics_self_advanced.py:44:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
 generics_self_advanced.py:45:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
-generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
-generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
-generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
+generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'> | None`, found `LinkedList[int]`
+generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `<special form 'typing.Self'> | None`
+generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `<special form 'typing.Self'> | None`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:27:9: error[type-assertion-failure] Type `type[Self@from_config]` does not match asserted type `Unknown`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
@@ -521,16 +521,16 @@
 generics_self_basic.py:67:26: error[invalid-type-form] Special form `typing.Self` expected no type parameter
 generics_self_protocols.py:61:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `BadReturnType`
 generics_self_protocols.py:64:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `ReturnDifferentClass`
-generics_self_usage.py:50:34: error[invalid-assignment] Object of type `def foo(self) -> int` is not assignable to `(typing.Self, /) -> int`
-generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:76:6: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
+generics_self_usage.py:50:34: error[invalid-assignment] Object of type `def foo(self) -> int` is not assignable to `(<special form 'typing.Self'>, /) -> int`
+generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `<special form 'typing.Self'>` is not allowed in a type expression
+generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `<special form 'typing.Self'>` is not allowed in a type expression
+generics_self_usage.py:76:6: error[invalid-type-form] Variable of type `<special form 'typing.Self'>` is not allowed in a type expression
 generics_self_usage.py:82:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@has_existing_self_annotation`
 generics_self_usage.py:86:16: error[invalid-return-type] Return type does not match returned value: expected `Self@return_concrete_type`, found `Foo3`
 generics_self_usage.py:98:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@Bar`
-generics_self_usage.py:101:15: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:103:12: error[invalid-base] Invalid class base with type `typing.Self`
-generics_self_usage.py:106:30: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
+generics_self_usage.py:101:15: error[invalid-type-form] Variable of type `<special form 'typing.Self'>` is not allowed in a type expression
+generics_self_usage.py:103:12: error[invalid-base] Invalid class base with type `<special form 'typing.Self'>`
+generics_self_usage.py:106:30: error[invalid-type-form] Variable of type `<special form 'typing.Self'>` is not allowed in a type expression
 generics_self_usage.py:111:19: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@make`
 generics_self_usage.py:116:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@return_parameter`
 generics_self_usage.py:121:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
@@ -861,11 +861,11 @@
 qualifiers_annotated.py:48:18: error[invalid-type-form] Boolean operations are not allowed in type expressions
 qualifiers_annotated.py:49:18: error[fstring-type-annotation] Type expressions cannot use f-strings
 qualifiers_annotated.py:59:8: error[invalid-type-form] Special form `typing.Annotated` expected at least 2 arguments (one type and at least one metadata element)
-qualifiers_annotated.py:71:24: error[invalid-assignment] Object of type `<typing.Annotated special form>` is not assignable to `type[Any]`
-qualifiers_annotated.py:72:24: error[invalid-assignment] Object of type `<typing.Annotated special form>` is not assignable to `type[Any]`
-qualifiers_annotated.py:79:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<typing.Annotated special form>`
-qualifiers_annotated.py:80:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<typing.Annotated special form>`
-qualifiers_annotated.py:86:1: error[call-non-callable] Object of type `typing.Annotated` is not callable
+qualifiers_annotated.py:71:24: error[invalid-assignment] Object of type `<special form 'typing.Annotated[int, <metadata>]'>` is not assignable to `type[Any]`
+qualifiers_annotated.py:72:24: error[invalid-assignment] Object of type `<special form 'typing.Annotated[int, <metadata>]'>` is not assignable to `type[Any]`
+qualifiers_annotated.py:79:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<special form 'typing.Annotated[str, <metadata>]'>`
+qualifiers_annotated.py:80:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<special form 'typing.Annotated[int, <metadata>]'>`
+qualifiers_annotated.py:86:1: error[call-non-callable] Object of type `<special form 'typing.Annotated'>` is not callable
 qualifiers_annotated.py:87:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_annotated.py:88:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
@@ -902,7 +902,7 @@
 specialtypes_none.py:32:1: error[missing-argument] No argument provided for required parameter `value` of function `__eq__`
 specialtypes_promotions.py:13:5: warning[possibly-missing-attribute] Attribute `numerator` may be missing on object of type `int | float`
 specialtypes_type.py:56:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[BasicUser] | type[ProUser]`, found `<class 'TeamUser'>`
-specialtypes_type.py:70:7: error[invalid-argument-type] Argument to function `func5` is incorrect: Expected `type[Unknown]`, found `typing.Callable`
+specialtypes_type.py:70:7: error[invalid-argument-type] Argument to function `func5` is incorrect: Expected `type[Unknown]`, found `<special form 'typing.Callable'>`
 specialtypes_type.py:76:17: error[invalid-type-form] type[...] must have exactly one type argument
 specialtypes_type.py:84:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:99:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-03 15:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.pyi:166:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co@_SizedIterable]'>]`
+ more_itertools/more.pyi:166:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[<special form 'typing.Protocol[_T_co]'>, <class 'Sized'>, <class 'Iterable[_T_co@_SizedIterable]'>]`
- more_itertools/more.pyi:169:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co@_SizedReversible]'>]`
+ more_itertools/more.pyi:169:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[<special form 'typing.Protocol[_T_co]'>, <class 'Sized'>, <class 'Reversible[_T_co@_SizedReversible]'>]`

anyio (https://github.com/agronholm/anyio)
- src/anyio/from_thread.py:123:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_BlockingAsyncContextManager` with bases list `[typing.Generic[T_co], <class 'AbstractContextManager'>]`
+ src/anyio/from_thread.py:123:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_BlockingAsyncContextManager` with bases list `[<special form 'typing.Generic[T_co]'>, <class 'AbstractContextManager'>]`

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/distro/distro.py:56:17: error[invalid-assignment] Object of type `<class 'dict'>` is not assignable to `typing.TypedDict`
+ lib/spack/spack/vendor/distro/distro.py:56:17: error[invalid-assignment] Object of type `<class 'dict'>` is not assignable to `<special form 'typing.TypedDict'>`
- lib/spack/spack/vendor/pyrsistent/typing.py:46:5: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `CheckedPSet` with bases list `[typing.Generic[T], <class 'Hashable'>]`
+ lib/spack/spack/vendor/pyrsistent/typing.py:46:5: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `CheckedPSet` with bases list `[<special form 'typing.Generic[T]'>, <class 'Hashable'>]`
- lib/spack/spack/vendor/pyrsistent/typing.py:65:5: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `PSet` with bases list `[typing.Generic[T], <class 'Hashable'>]`
+ lib/spack/spack/vendor/pyrsistent/typing.py:65:5: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `PSet` with bases list `[<special form 'typing.Generic[T]'>, <class 'Hashable'>]`
- lib/spack/spack/vendor/pyrsistent/typing.pyi:118:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `PSetEvolver` with bases list `[typing.Generic[T], <class 'Sized'>]`
+ lib/spack/spack/vendor/pyrsistent/typing.pyi:118:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `PSetEvolver` with bases list `[<special form 'typing.Generic[T]'>, <class 'Sized'>]`
- lib/spack/spack/vendor/pyrsistent/typing.pyi:126:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `PBag` with bases list `[typing.Generic[T], <class 'Sized'>, <class 'Hashable'>]`
+ lib/spack/spack/vendor/pyrsistent/typing.pyi:126:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `PBag` with bases list `[<special form 'typing.Generic[T]'>, <class 'Sized'>, <class 'Hashable'>]`
- lib/spack/spack/vendor/typing_extensions.py:1037:25: warning[unsupported-base] Unsupported class base with type `typing.Protocol | <class 'Protocol'>`
+ lib/spack/spack/vendor/typing_extensions.py:1037:25: warning[unsupported-base] Unsupported class base with type `<special form 'typing.Protocol'> | <class 'Protocol'>`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/distro/distro.py:56:17: error[invalid-assignment] Object of type `<class 'dict'>` is not assignable to `typing.TypedDict`
+ src/pip/_vendor/distro/distro.py:56:17: error[invalid-assignment] Object of type `<class 'dict'>` is not assignable to `<special form 'typing.TypedDict'>`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `UpdateDictMixin[Any, Any]`
+ src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'>`, found `UpdateDictMixin[Any, Any]`
- src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `Self@setdefault`
+ src/werkzeug/datastructures/mixins.py:262:28: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'>`, found `Self@setdefault`
- src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `Self@pop`
+ src/werkzeug/datastructures/mixins.py:282:28: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'>`, found `Self@pop`
- src/werkzeug/datastructures/structures.py:1053:9: error[invalid-assignment] Object of type `((Self@__init__, /) -> None) | None` is not assignable to attribute `on_update` of type `((typing.Self, /) -> None) | None`
+ src/werkzeug/datastructures/structures.py:1053:9: error[invalid-assignment] Object of type `((Self@__init__, /) -> None) | None` is not assignable to attribute `on_update` of type `((<special form 'typing.Self'>, /) -> None) | None`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/ntlm_upstream_proxy.py:38:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `types.UnionType`
+ examples/contrib/ntlm_upstream_proxy.py:38:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `<types.UnionType special form 'str | None'>`
- examples/contrib/ntlm_upstream_proxy.py:47:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `types.UnionType`
+ examples/contrib/ntlm_upstream_proxy.py:47:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `<types.UnionType special form 'str | None'>`
- examples/contrib/ntlm_upstream_proxy.py:55:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `types.UnionType`
+ examples/contrib/ntlm_upstream_proxy.py:55:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `<types.UnionType special form 'str | None'>`
- test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = typing.Any) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = <special form 'typing.Any'>) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
- test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = typing.Any) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[invalid-type-form] Invalid subscript of object of type `def Placeholder[T](cls: type[T@Placeholder] = <special form 'typing.Any'>) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
- test/mitmproxy/proxy/tutils.py:405:17: error[invalid-parameter-default] Default value of type `typing.Any` is not assignable to annotated parameter type `type[T@Placeholder]`
+ test/mitmproxy/proxy/tutils.py:405:17: error[invalid-parameter-default] Default value of type `<special form 'typing.Any'>` is not assignable to annotated parameter type `type[T@Placeholder]`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:1601:12: error[invalid-return-type] Return type does not match returned value: expected `type[_T@_annotated]`, found `<typing.Annotated special form>`
+ tanjun/annotations.py:1601:12: error[invalid-return-type] Return type does not match returned value: expected `type[_T@_annotated]`, found `<special form 'typing.Annotated[Unknown, <metadata>]'>`
- tanjun/annotations.py:1855:16: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `<typing.Annotated special form>`
+ tanjun/annotations.py:1855:16: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `<special form 'typing.Annotated[str, <metadata>]'>`
- tanjun/annotations.py:2268:16: error[invalid-return-type] Return type does not match returned value: expected `type[PartialChannel]`, found `<typing.Annotated special form>`
+ tanjun/annotations.py:2268:16: error[invalid-return-type] Return type does not match returned value: expected `type[PartialChannel]`, found `<special form 'typing.Annotated[PartialChannel, <metadata>]'>`
- tanjun/annotations.py:2520:30: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `Self@add_to_slash_cmds`
+ tanjun/annotations.py:2520:30: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'>`, found `Self@add_to_slash_cmds`

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/typing.pyi:408:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Any | None`, found `GenericAlias`
+ mypy/typeshed/stdlib/typing.pyi:408:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Any | None`, found `<special form 'type[object]'>`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/cog.py:288:36: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Cog | None` of type variable `CogT@Command`
+ discord/ext/commands/cog.py:288:36: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Cog | None` of type variable `CogT@Command`
- discord/ext/commands/cog.py:289:79: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Group | Cog` of type variable `GroupT@Command`
+ discord/ext/commands/cog.py:289:79: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Group | Cog` of type variable `GroupT@Command`
- discord/ui/action_row.py:122:67: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `BaseView | ActionRow[Unknown] | Container[Unknown]` of type variable `C@ContainedItemCallbackType`
+ discord/ui/action_row.py:122:67: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `BaseView | ActionRow[Unknown] | Container[Unknown]` of type variable `C@ContainedItemCallbackType`
- discord/ui/container.py:109:77: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `BaseView | ActionRow[Unknown] | Container[Unknown]` of type variable `C@ContainedItemCallbackType`
+ discord/ui/container.py:109:77: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `BaseView | ActionRow[Unknown] | Container[Unknown]` of type variable `C@ContainedItemCallbackType`
- discord/ui/modal.py:109:55: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `BaseView` of type variable `V@Item`
+ discord/ui/modal.py:109:55: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `BaseView` of type variable `V@Item`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/_typing.py:27:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `SizedStringProtocol` with bases list `[typing.Protocol, <class 'StringProtocol'>, <class 'Sized'>]`
+ mesonbuild/_typing.py:27:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `SizedStringProtocol` with bases list `[<special form 'typing.Protocol'>, <class 'StringProtocol'>, <class 'Sized'>]`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/more_itertools/more.pyi:38:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co@_SizedIterable]'>]`
+ setuptools/_vendor/more_itertools/more.pyi:38:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[<special form 'typing.Protocol[_T_co]'>, <class 'Sized'>, <class 'Iterable[_T_co@_SizedIterable]'>]`
- setuptools/_vendor/more_itertools/more.pyi:41:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co@_SizedReversible]'>]`
+ setuptools/_vendor/more_itertools/more.pyi:41:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[<special form 'typing.Protocol[_T_co]'>, <class 'Sized'>, <class 'Reversible[_T_co@_SizedReversible]'>]`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/transactions.py:218:30: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `typing.Self`, found `Self@add_child`
+ src/prefect/transactions.py:218:30: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `<special form 'typing.Self'>`, found `Self@add_child`
- src/prefect/transactions.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `Self@get_active | None`, found `None | Self@get_active | typing.Self`
+ src/prefect/transactions.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `Self@get_active | None`, found `None | Self@get_active | <special form 'typing.Self'>`
- src/prefect/transactions.py:267:40: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `typing.Self`, found `Self@__enter__`
+ src/prefect/transactions.py:267:40: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `<special form 'typing.Self'>`, found `Self@__enter__`
- src/prefect/transactions.py:614:40: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `typing.Self`, found `Self@__aenter__`
+ src/prefect/transactions.py:614:40: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `<special form 'typing.Self'>`, found `Self@__aenter__`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/utils/typing.py:339:12: error[invalid-return-type] Return type does not match returned value: expected `type`, found `@Todo | typing.Union`
+ strawberry/utils/typing.py:339:12: error[invalid-return-type] Return type does not match returned value: expected `type`, found `@Todo | <special form 'typing.Union'>`

xarray (https://github.com/pydata/xarray)
- xarray/core/treenode.py:88:16: error[invalid-return-type] Return type does not match returned value: expected `Self@parent | None`, found `typing.Self | None`
+ xarray/core/treenode.py:88:16: error[invalid-return-type] Return type does not match returned value: expected `Self@parent | None`, found `<special form 'typing.Self'> | None`
- xarray/core/treenode.py:133:13: error[invalid-assignment] Object of type `dict[str | Unknown, Self@_detach | Unknown]` is not assignable to attribute `_children` of type `dict[str, typing.Self]`
+ xarray/core/treenode.py:133:13: error[invalid-assignment] Object of type `dict[str | Unknown, Self@_detach | Unknown]` is not assignable to attribute `_children` of type `dict[str, <special form 'typing.Self'>]`
- xarray/core/treenode.py:153:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `Self@_attach` on object of type `dict[str, typing.Self]`
+ xarray/core/treenode.py:153:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `Self@_attach` on object of type `dict[str, <special form 'typing.Self'>]`
- xarray/core/treenode.py:154:13: error[invalid-assignment] Object of type `Self@_attach` is not assignable to attribute `_parent` of type `typing.Self | None`
+ xarray/core/treenode.py:154:13: error[invalid-assignment] Object of type `Self@_attach` is not assignable to attribute `_parent` of type `<special form 'typing.Self'> | None`
- xarray/core/treenode.py:166:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[str, Self@children]`, found `Frozen[str, typing.Self]`
+ xarray/core/treenode.py:166:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[str, Self@children]`, found `Frozen[str, <special form 'typing.Self'>]`

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/contrib/admin/options.pyi:175:39: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `ModelAdmin[Any]` of type variable `_ModelAdmin`
+ django-stubs/contrib/admin/options.pyi:175:39: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `ModelAdmin[Any]` of type variable `_ModelAdmin`
- django-stubs/contrib/auth/models.pyi:106:35: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `AbstractBaseUser` of type variable `_UserType@UserManager`
+ django-stubs/contrib/auth/models.pyi:106:35: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `AbstractBaseUser` of type variable `_UserType@UserManager`
- django-stubs/contrib/gis/db/backends/oracle/models.pyi:12:31: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/contrib/gis/db/backends/oracle/models.pyi:12:31: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/contrib/gis/db/backends/oracle/models.pyi:26:31: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/contrib/gis/db/backends/oracle/models.pyi:26:31: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/contrib/gis/db/backends/postgis/models.pyi:15:38: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/contrib/gis/db/backends/postgis/models.pyi:15:38: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/contrib/gis/db/backends/postgis/models.pyi:28:38: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/contrib/gis/db/backends/postgis/models.pyi:28:38: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/contrib/gis/db/backends/spatialite/models.pyi:14:38: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/contrib/gis/db/backends/spatialite/models.pyi:14:38: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/contrib/gis/db/backends/spatialite/models.pyi:28:38: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/contrib/gis/db/backends/spatialite/models.pyi:28:38: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/contrib/sessions/base_session.pyi:18:42: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `AbstractBaseSession` of type variable `_T@BaseSessionManager`
+ django-stubs/contrib/sessions/base_session.pyi:18:42: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `AbstractBaseSession` of type variable `_T@BaseSessionManager`
- django-stubs/core/paginator.pyi:14:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SupportsPagination` with bases list `[typing.Protocol[_T], <class 'Sized'>, <class 'Iterable'>]`
+ django-stubs/core/paginator.pyi:14:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SupportsPagination` with bases list `[<special form 'typing.Protocol[_T]'>, <class 'Sized'>, <class 'Iterable'>]`
- django-stubs/db/migrations/recorder.pyi:14:35: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/db/migrations/recorder.pyi:14:35: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/db/models/base.pyi:45:31: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_T@Manager`
+ django-stubs/db/models/base.pyi:45:31: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_T@Manager`
- django-stubs/db/models/base.pyi:47:29: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `Model` of type variable `_M@Options`
+ django-stubs/db/models/base.pyi:47:29: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `Model` of type variable `_M@Options`
- django-stubs/utils/datastructures.pyi:39:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_IndexableCollection` with bases list `[typing.Protocol[_I], <class 'Collection[_I@_IndexableCollection]'>]`
+ django-stubs/utils/datastructures.pyi:39:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_IndexableCollection` with bases list `[<special form 'typing.Protocol[_I]'>, <class 'Collection[_I@_IndexableCollection]'>]`

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/impala/tests/test_udf.py:388:28: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `typing.Literal`
+ ibis/backends/impala/tests/test_udf.py:388:28: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `<special form 'typing.Literal'>`
- ibis/common/patterns.py:165:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type | tuple[type, ...]`, found `typing.Callable`
+ ibis/common/patterns.py:165:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type | tuple[type, ...]`, found `<special form 'typing.Callable'>`
- ibis/common/tests/test_grounds.py:332:26: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `RecursiveNode`
+ ibis/common/tests/test_grounds.py:332:26: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'> | None`, found `RecursiveNode`
- ibis/common/tests/test_grounds.py:332:40: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `RecursiveNode`
+ ibis/common/tests/test_grounds.py:332:40: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'> | None`, found `RecursiveNode`
- ibis/common/tests/test_grounds.py:751:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type | tuple[type, ...]`, found `typing.Callable`
+ ibis/common/tests/test_grounds.py:751:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type | tuple[type, ...]`, found `<special form 'typing.Callable'>`
- ibis/common/tests/test_patterns.py:731:31: error[invalid-argument-type] Argument to bound method `from_typehint` is incorrect: Expected `type`, found `GenericAlias`
+ ibis/common/tests/test_patterns.py:731:31: error[invalid-argument-type] Argument to bound method `from_typehint` is incorrect: Expected `type`, found `<typing.Callable special form '(int, str, /) -> int'>`
- ibis/common/tests/test_patterns.py:1125:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type | tuple[type, ...]`, found `typing.Callable`
+ ibis/common/tests/test_patterns.py:1125:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type | tuple[type, ...]`, found `<special form 'typing.Callable'>`
- ibis/expr/datatypes/tests/test_core.py:719:31: error[invalid-argument-type] Argument to bound method `from_typehint` is incorrect: Expected `type`, found `<typing.Annotated special form>`
+ ibis/expr/datatypes/tests/test_core.py:719:31: error[invalid-argument-type] Argument to bound method `from_typehint` is incorrect: Expected `type`, found `<special form 'typing.Annotated[Interval, <metadata>]'>`
- ibis/expr/types/logical.py:333:18: error[call-non-callable] Object of type `typing.Any` is not callable
+ ibis/expr/types/logical.py:333:18: error[call-non-callable] Object of type `<special form 'typing.Any'>` is not callable
- ibis/expr/types/relations.py:2100:38: error[invalid-argument-type] Argument to bound method `_assemble_set_op` is incorrect: Expected `type[Set]`, found `typing.Union`
+ ibis/expr/types/relations.py:2100:38: error[invalid-argument-type] Argument to bound method `_assemble_set_op` is incorrect: Expected `type[Set]`, found `<special form 'typing.Union'>`
- ibis/tests/expr/test_table.py:1360:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `typing.Union`
+ ibis/tests/expr/test_table.py:1360:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `<special form 'typing.Union'>`
- ibis/tests/expr/test_window_frames.py:540:18: error[call-non-callable] Object of type `typing.Any` is not callable
+ ibis/tests/expr/test_window_frames.py:540:18: error[call-non-callable] Object of type `<special form 'typing.Any'>` is not callable

sympy (https://github.com/sympy/sympy)
- sympy/polys/domains/gaussiandomains.py:33:29: error[invalid-type-arguments] Type `typing.Self` is not assignable to upper bound `GaussianElement[Unknown]` of type variable `Telem@GaussianDomain`
+ sympy/polys/domains/gaussiandomains.py:33:29: error[invalid-type-arguments] Type `<special form 'typing.Self'>` is not assignable to upper bound `GaussianElement[Unknown]` of type variable `Telem@GaussianDomain`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5512 diagnostics
+ Found 5511 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/ring/switch.py:103:38: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `RingSwitchEntityDescription[Any]`
+ homeassistant/components/ring/switch.py:103:38: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'>`, found `RingSwitchEntityDescription[Any]`
- homeassistant/components/ring/switch.py:124:57: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self`, found `RingSwitchEntityDescription[RingDeviceT@RingSwitch]`
+ homeassistant/components/ring/switch.py:124:57: error[invalid-argument-type] Argument is incorrect: Expected `<special form 'typing.Self'>`, found `RingSwitchEntityDescription[RingDeviceT@RingSwitch]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_generate_schema.py:131:27: error[invalid-assignment] Object of type `list[type | typing.Tuple]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:131:27: error[invalid-assignment] Object of type `list[type | <special form 'typing.Tuple'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:132:26: error[invalid-assignment] Object of type `list[type | typing.List]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:132:26: error[invalid-assignment] Object of type `list[type | <special form 'typing.List'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:133:25: error[invalid-assignment] Object of type `list[type | typing.Set]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:133:25: error[invalid-assignment] Object of type `list[type | <special form 'typing.Set'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:134:32: error[invalid-assignment] Object of type `list[type | typing.FrozenSet]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:134:32: error[invalid-assignment] Object of type `list[type | <special form 'typing.FrozenSet'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:135:26: error[invalid-assignment] Object of type `list[type | typing.Dict]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:135:26: error[invalid-assignment] Object of type `list[type | <special form 'typing.Dict'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:139:26: error[invalid-assignment] Object of type `list[type | typing.Type]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:139:26: error[invalid-assignment] Object of type `list[type | <special form 'typing.Type'>]` is not assignable to `list[type]`
- pydantic/_internal/_generate_schema.py:160:27: error[invalid-assignment] Object of type `list[type | typing.Deque]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:160:27: error[invalid-assignment] Object of type `list[type | <special form 'typing.Deque'>]` is not assignable to `list[type]`
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- pydantic/functional_validators.py:781:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/functional_validators.py:781:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<special form 'typing.Annotated[Unknown, <metadata>]'>`
- pydantic/json_schema.py:2827:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/json_schema.py:2827:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<special form 'typing.Annotated[Unknown, <metadata>]'>`
- pydantic/types.py:230:12: error[invalid-return-type] Return type does not match returned value: expected `type[int]`, found `<typing.Annotated special form>`
+ pydantic/types.py:230:12: error[invalid-return-type] Return type does not match returned value: expected `type[int]`, found `<special form 'typing.Annotated[int, <metadata>]'>`
- pydantic/types.py:679:12: error[invalid-return-type] Return type does not match returned value: expected `type[bytes]`, found `<typing.Annotated special form>`
+ pydantic/types.py:679:12: error[invalid-return-type] Return type does not match returned value: expected `type[bytes]`, found `<special form 'typing.Annotated[bytes, <metadata>]'>`
- pydantic/types.py:817:12: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `<typing.Annotated special form>`
+ pydantic/types.py:817:12: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `<special form 'typing.Annotated[str, <metadata>]'>`
- pydantic/types.py:852:12: error[invalid-return-type] Return type does not match returned value: expected `type[set[HashableItemType@conset]]`, found `<typing.Annotated special form>`
+ pydantic/types.py:852:12: error[invalid-return-type] Return type does not match returned value: expected `type[set[HashableItemType@conset]]`, found `<special form 'typing.Annotated[set[Unknown], <metadata>]'>`
- pydantic/types.py:868:12: error[invalid-return-type] Return type does not match returned value: expected `type[frozenset[HashableItemType@confrozenset]]`, found `<typing.Annotated special form>`
+ pydantic/types.py:868:12: error[invalid-return-type] Return type does not match returned value: expected `type[frozenset[HashableItemType@confrozenset]]`, found `<special form 'typing.Annotated[frozenset[Unknown], <metadata>]'>`
- pydantic/types.py:903:12: error[invalid-return-type] Return type does not match returned value: expected `type[list[AnyItemType@conlist]]`, found `<typing.Annotated special form>`
+ pydantic/types.py:903:12: error[invalid-return-type] Return type does not match returned value: expected `type[list[AnyItemType@conlist]]`, found `<special form 'typing.Annotated[list[Unknown], <metadata>]'>`
- pydantic/types.py:995:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/types.py:995:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<special form 'typing.Annotated[Unknown, <metadata>]'>`
- pydantic/types.py:1124:12: error[invalid-return-type] Return type does not match returned value: expected `type[Decimal]`, found `<typing.Annotated special form>`
+ pydantic/types.py:1124:12: error[invalid-return-type] Return type does not match returned value: expected `type[Decimal]`, found `<special form 'typing.Annotated[Decimal, <metadata>]'>`
- pydantic/types.py:1511:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/types.py:1511:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<special form 'typing.Annotated[Unknown, <metadata>]'>`
- pydantic/types.py:2257:12: error[invalid-return-type] Return type does not match returned value: expected `type[date]`, found `<typing.Annotated special form>`
+ pydantic/types.py:2257:12: error[invalid-return-type] Return type does not match returned value: expected `type[date]`, found `<special form 'typing.Annotated[date, <metadata>]'>`
- pydantic/v1/fields.py:586:45: error[invalid-argument-type] Argument to function `lenient_issubclass` is incorrect: Expected `type[Any] | tuple[type[Any], ...] | None`, found `<typing.Annotated special form>`
+ pydantic/v1/fields.py:586:45: error[invalid-argument-type] Argument to function `lenient_issubclass` is incorrect: Expected `type[Any] | tuple[type[Any], ...] | None`, found `<special form 'typing.Annotated[T@Json, <metadata>]'>`
- pydantic/v1/types.py:832:20: error[invalid-return-type] Return type does not match returned value: expected `type[JsonWrapper]`, found `<typing.Annotated special form>`
+ pydantic/v1/types.py:832:20: error[invalid-return-type] Return type does not match returned value: expected `type[JsonWrapper]`, found `<special form 'typing.Annotated[T@Json, <metadata>]'>`


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-12-03 16:00_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-03 16:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-03 16:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-03 16:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-03 16:00_

---

_Comment by @AlexWaygood on 2025-12-03 16:07_

(I also made some changes to make clear that the objects `typing.Literal` and `typing.Literal[42]`, for example, inhabit different types in our model, and are not fungible!)

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:65 on 2025-12-03 18:44_

Should the PEP 695 versions also have backticks, parentheses, or both?
```suggestion
reveal_type(C[int, int])  # revealed: <type alias (`C[Unknown]`)>
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:117 on 2025-12-03 18:48_

I thought about doing this. I like it personally, but it goes a bit against the goal you're trying to achieve here? When users see the type of `IntOrStr` somewhere in a diagnostic, there's a high likelihood that they've done something wrong (?), so maybe displaying `<special form types.UnionType>` makes that even clearer, given that `int | str` is probably what they really wanted?

---

_@sharkdp approved on 2025-12-03 18:49_

Cool, thank you!

Do we need to worry about the fact that things like `<special form typing.Never>` are not valid Python syntax? Do we show the display types syntax-highlighted somewhere?

---

_@AlexWaygood reviewed on 2025-12-03 18:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:117 on 2025-12-03 18:53_

That's true, but i worry that that display implies that all `UnionType` value expressions inhabit the same type? When in fact, in our model, each has its own bespoke type. I would say part of the aim of this PR is to more clearly differentiate different types by giving them clearly different displays!

---

_Comment by @AlexWaygood on 2025-12-03 20:43_

> Do we need to worry about the fact that things like `<special form typing.Never>` are not valid Python syntax? Do we show the display types syntax-highlighted somewhere?

I've done `f.set_invalid_syntax()` for the display of each of these, so I think that _should_ be enough to notify the LSP that it shouldn't try to apply Python syntax highlighting to these. And we use similar displays for other types too, obviously, such as `<class 'Foo'>`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:65 on 2025-12-03 20:45_

Hmm, yeah, it's a bit inconsistent. Thanks. I think I'm actually going to inflict maximum pain on myself and change them both to be more consistent with our class-literal/generic-alias display, which looks like

```py
<class 'Foo'>
<class 'Bar[int]'>
```

etc.

(AKA, single quotes instead of backticks, and no parentheses)

---

_@AlexWaygood reviewed on 2025-12-03 20:45_

---

_Merged by @AlexWaygood on 2025-12-03 21:20_

---

_Closed by @AlexWaygood on 2025-12-03 21:20_

---

_Branch deleted on 2025-12-03 21:20_

---
