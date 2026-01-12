```yaml
number: 21498
title: "[ty] Custom concise diagnostic messages"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/custom-concise-diagnostic
created_at: 2025-11-17T09:29:56Z
updated_at: 2025-11-18T13:58:37Z
url: https://github.com/astral-sh/ruff/pull/21498
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Custom concise diagnostic messages

---

_@sharkdp_

## Summary

This PR proposes that we add a new `set_concise_message` functionality to our `Diagnostic` construction API. When used, the concise message that is otherwise auto-generated from the main diagnostic message and the primary annotation will be overwritten with the custom message.

To understand why this is desirable, let's look at the `invalid-key` diagnostic. This is how I *want* the full diagnostic to look like:

<img width="620" height="282" alt="image" src="https://github.com/user-attachments/assets/3bf70f52-9d9f-4817-bc16-fb0ebf7c2113" />

However, without the change in this PR, the concise message would have the following form:

```
error[invalid-key]: Unknown key "Age" for TypedDict `Person`: Unknown key "Age" - did you mean "age"?
```

This duplication is why the full `invalid-key` diagnostic used a main diagnostic message that is only "Invalid key for TypedDict `Person`", to make that bearable:

```
error[invalid-key] Invalid key for TypedDict `Person`: Unknown key "Age" - did you mean "age"?
```

This is still less than ideal, *and* we had to make the "full" diagnostic worse. With the new API here, we have to make no such compromises. We need to do slightly more work (provide one additional custom-designed message), but we get to keep the "full" diagnostic that we actually want, and we can make the concise message more terse and readable:

```
error[invalid-key] Unknown key "Age" for TypedDict `Person` - did you mean "age"?
```

Similar problems exist for other diagnostics as well (I really want this for https://github.com/astral-sh/ruff/pull/21476). In this PR, I only changed `invalid-key` and `type-assertion-failure`.

The PR here is somewhat related to the discussion in https://github.com/astral-sh/ty/issues/1418, but note that we are solving a problem that is unrelated to sub-diagnostics.

## Test Plan

Updated tests


---

_Label `ty` added by @sharkdp on 2025-11-17 09:29_

---

_Label `diagnostics` added by @sharkdp on 2025-11-17 09:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 09:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-18 08:17:47.816286467 +0000
+++ new-output.txt	2025-11-18 08:17:51.288307405 +0000
@@ -6,22 +6,22 @@
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_explicit.py:45:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
-aliases_explicit.py:52:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
-aliases_explicit.py:53:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
-aliases_explicit.py:54:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_explicit.py:56:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
-aliases_explicit.py:57:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
-aliases_explicit.py:59:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
-aliases_explicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
+aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_explicit.py:53:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
+aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_explicit.py:56:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
+aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
+aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(unknown type subscript)]`
+aliases_explicit.py:61:5: error[type-assertion-failure] Type `int | str` does not match asserted type `Unknown`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
-aliases_implicit.py:64:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
-aliases_implicit.py:65:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_implicit.py:67:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
-aliases_implicit.py:68:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
-aliases_implicit.py:70:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Argument does not have asserted type `list[bool]`
+aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_implicit.py:64:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
+aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_implicit.py:67:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
+aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
+aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(unknown type subscript)]`
+aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(unknown type subscript)`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[<class 'int'> | Unknown]` is not allowed in a type expression
@@ -83,18 +83,18 @@
 annotations_forward_refs.py:82:11: error[invalid-type-form] Variable of type `Literal[""]` is not allowed in a type expression
 annotations_forward_refs.py:87:9: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
 annotations_forward_refs.py:89:8: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
-annotations_forward_refs.py:95:1: error[type-assertion-failure] Argument does not have asserted type `str`
-annotations_forward_refs.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
+annotations_forward_refs.py:95:1: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
+annotations_forward_refs.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 annotations_generators.py:86:21: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.GeneratorType`
 annotations_generators.py:91:27: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.AsyncGeneratorType`
-annotations_generators.py:193:1: error[type-assertion-failure] Argument does not have asserted type `() -> AsyncIterator[int]`
-annotations_methods.py:31:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_methods.py:36:1: error[type-assertion-failure] Argument does not have asserted type `B`
-annotations_methods.py:42:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_methods.py:48:1: error[type-assertion-failure] Argument does not have asserted type `A`
-annotations_methods.py:49:1: error[type-assertion-failure] Argument does not have asserted type `B`
-annotations_methods.py:50:1: error[type-assertion-failure] Argument does not have asserted type `B`
-annotations_methods.py:51:1: error[type-assertion-failure] Argument does not have asserted type `A`
+annotations_generators.py:193:1: error[type-assertion-failure] Type `() -> AsyncIterator[int]` does not match asserted type `def generator30() -> AsyncIterator[int]`
+annotations_methods.py:31:1: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
+annotations_methods.py:36:1: error[type-assertion-failure] Type `B` does not match asserted type `Unknown`
+annotations_methods.py:42:1: error[type-assertion-failure] Type `A` does not match asserted type `B`
+annotations_methods.py:48:1: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
+annotations_methods.py:49:1: error[type-assertion-failure] Type `B` does not match asserted type `Unknown`
+annotations_methods.py:50:1: error[type-assertion-failure] Type `B` does not match asserted type `Unknown`
+annotations_methods.py:51:1: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
 annotations_typeexpr.py:88:9: error[invalid-type-form] Function calls are not allowed in type expressions
 annotations_typeexpr.py:89:9: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 annotations_typeexpr.py:90:9: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
@@ -124,10 +124,10 @@
 callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
 callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
 callables_annotation.py:157:5: error[invalid-assignment] Object of type `Proto7` is not assignable to `Proto6`
-callables_kwargs.py:24:5: error[type-assertion-failure] Argument does not have asserted type `int`
-callables_kwargs.py:32:9: error[type-assertion-failure] Argument does not have asserted type `str`
-callables_kwargs.py:35:5: error[type-assertion-failure] Argument does not have asserted type `str`
-callables_kwargs.py:41:5: error[type-assertion-failure] Argument does not have asserted type `TD1`
+callables_kwargs.py:24:5: error[type-assertion-failure] Type `int` does not match asserted type `@Todo(`Unpack[]` special form)`
+callables_kwargs.py:32:9: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
+callables_kwargs.py:35:5: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
+callables_kwargs.py:41:5: error[type-assertion-failure] Type `TD1` does not match asserted type `dict[str, @Todo(`Unpack[]` special form)]`
 callables_kwargs.py:52:11: error[too-many-positional-arguments] Too many positional arguments to function `func1`: expected 0, got 3
 callables_kwargs.py:64:11: error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `str`, found `Literal[1]`
 callables_kwargs.py:64:14: error[parameter-already-assigned] Multiple values provided for parameter `v3` of function `func2`
@@ -194,61 +194,61 @@
 classes_classvar.py:111:1: error[invalid-attribute-access] Cannot assign to ClassVar `stats` from an instance of type `Starship`
 classes_classvar.py:140:1: error[invalid-assignment] Object of type `ProtoAImpl` is not assignable to `ProtoA`
 constructors_call_init.py:21:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `float`
-constructors_call_init.py:25:1: error[type-assertion-failure] Argument does not have asserted type `Class1[int | float]`
+constructors_call_init.py:25:1: error[type-assertion-failure] Type `Class1[int | float]` does not match asserted type `Class1[float]`
 constructors_call_init.py:56:1: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Class4[int]`, found `Class4[str]`
-constructors_call_init.py:75:1: error[type-assertion-failure] Argument does not have asserted type `Class5[int | float]`
-constructors_call_init.py:91:1: error[type-assertion-failure] Argument does not have asserted type `Class6[int, str]`
-constructors_call_init.py:99:1: error[type-assertion-failure] Argument does not have asserted type `Class7[str, int]`
+constructors_call_init.py:75:1: error[type-assertion-failure] Type `Class5[int | float]` does not match asserted type `Class5[float]`
+constructors_call_init.py:91:1: error[type-assertion-failure] Type `Class6[int, str]` does not match asserted type `Class6[Unknown, Unknown]`
+constructors_call_init.py:99:1: error[type-assertion-failure] Type `Class7[str, int]` does not match asserted type `Class7[Unknown, Unknown]`
 constructors_call_init.py:130:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
-constructors_call_metaclass.py:39:1: error[type-assertion-failure] Argument does not have asserted type `int | Meta2`
+constructors_call_metaclass.py:39:1: error[type-assertion-failure] Type `int | Meta2` does not match asserted type `Class2`
 constructors_call_metaclass.py:39:13: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_metaclass.py:54:1: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_metaclass.py:68:1: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_new.py:21:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `int`, found `float`
-constructors_call_new.py:24:1: error[type-assertion-failure] Argument does not have asserted type `Class1[int | float]`
-constructors_call_new.py:49:1: error[type-assertion-failure] Argument does not have asserted type `int`
+constructors_call_new.py:24:1: error[type-assertion-failure] Type `Class1[int | float]` does not match asserted type `Class1[float]`
+constructors_call_new.py:49:1: error[type-assertion-failure] Type `int` does not match asserted type `Class3`
 constructors_call_new.py:49:13: error[missing-argument] No argument provided for required parameter `x` of bound method `__init__`
-constructors_call_new.py:64:1: error[type-assertion-failure] Argument does not have asserted type `Class4 | Any`
+constructors_call_new.py:64:1: error[type-assertion-failure] Type `Class4 | Any` does not match asserted type `Class4`
 constructors_call_new.py:64:13: error[missing-argument] No argument provided for required parameter `x` of bound method `__init__`
-constructors_call_new.py:76:5: error[type-assertion-failure] Argument does not have asserted type `Never`
+constructors_call_new.py:76:5: error[type-assertion-failure] Type `Never` does not match asserted type `Class5`
 constructors_call_new.py:76:17: error[missing-argument] No argument provided for required parameter `x` of bound method `__init__`
-constructors_call_new.py:89:1: error[type-assertion-failure] Argument does not have asserted type `int | Class6`
+constructors_call_new.py:89:1: error[type-assertion-failure] Type `int | Class6` does not match asserted type `Class6`
 constructors_call_new.py:89:13: error[missing-argument] No argument provided for required parameter `x` of bound method `__init__`
 constructors_call_new.py:113:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class8[list[T@Class8]]`
-constructors_call_new.py:116:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[int]]`
-constructors_call_new.py:117:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[str]]`
+constructors_call_new.py:116:1: error[type-assertion-failure] Type `Class8[list[int]]` does not match asserted type `Class8[int]`
+constructors_call_new.py:117:1: error[type-assertion-failure] Type `Class8[list[str]]` does not match asserted type `Class8[str]`
 constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 constructors_call_new.py:140:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class11[int]`
 constructors_call_type.py:40:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of function `__new__`
 constructors_call_type.py:50:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of bound method `__init__`
 constructors_call_type.py:59:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
 constructors_callable.py:36:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:37:1: error[type-assertion-failure] Argument does not have asserted type `Class1`
+constructors_callable.py:37:1: error[type-assertion-failure] Type `Class1` does not match asserted type `Unknown`
 constructors_callable.py:49:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:50:1: error[type-assertion-failure] Argument does not have asserted type `Class2`
+constructors_callable.py:50:1: error[type-assertion-failure] Type `Class2` does not match asserted type `Unknown`
 constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:64:1: error[type-assertion-failure] Argument does not have asserted type `Class3`
+constructors_callable.py:64:1: error[type-assertion-failure] Type `Class3` does not match asserted type `Unknown`
 constructors_callable.py:73:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 constructors_callable.py:77:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:78:1: error[type-assertion-failure] Argument does not have asserted type `int`
+constructors_callable.py:78:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 constructors_callable.py:97:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:100:5: error[type-assertion-failure] Argument does not have asserted type `Never`
-constructors_callable.py:105:5: error[type-assertion-failure] Argument does not have asserted type `Never`
+constructors_callable.py:100:5: error[type-assertion-failure] Type `Never` does not match asserted type `Unknown`
+constructors_callable.py:105:5: error[type-assertion-failure] Type `Never` does not match asserted type `Unknown`
 constructors_callable.py:125:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:126:1: error[type-assertion-failure] Argument does not have asserted type `Class6Proxy`
+constructors_callable.py:126:1: error[type-assertion-failure] Type `Class6Proxy` does not match asserted type `Unknown`
 constructors_callable.py:142:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:162:5: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:164:1: error[type-assertion-failure] Argument does not have asserted type `Class7[int]`
-constructors_callable.py:165:1: error[type-assertion-failure] Argument does not have asserted type `Class7[str]`
+constructors_callable.py:164:1: error[type-assertion-failure] Type `Class7[int]` does not match asserted type `Unknown`
+constructors_callable.py:165:1: error[type-assertion-failure] Type `Class7[str]` does not match asserted type `Unknown`
 constructors_callable.py:182:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:183:1: error[type-assertion-failure] Argument does not have asserted type `Class8[str]`
+constructors_callable.py:183:1: error[type-assertion-failure] Type `Class8[str]` does not match asserted type `Unknown`
 constructors_callable.py:193:13: info[revealed-type] Revealed type: `(...) -> Unknown`
-constructors_callable.py:194:1: error[type-assertion-failure] Argument does not have asserted type `Class9`
+constructors_callable.py:194:1: error[type-assertion-failure] Type `Class9` does not match asserted type `Unknown`
 dataclasses_descriptors.py:23:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | Desc1`
 dataclasses_descriptors.py:50:63: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[T@Desc2] | T@Desc2`
-dataclasses_descriptors.py:66:1: error[type-assertion-failure] Argument does not have asserted type `int`
-dataclasses_descriptors.py:67:1: error[type-assertion-failure] Argument does not have asserted type `str`
+dataclasses_descriptors.py:66:1: error[type-assertion-failure] Type `int` does not match asserted type `int | Desc2[int]`
+dataclasses_descriptors.py:67:1: error[type-assertion-failure] Type `str` does not match asserted type `str | Desc2[str]`
 dataclasses_final.py:27:1: error[invalid-assignment] Cannot assign to final attribute `final_classvar` on type `<class 'D'>`
 dataclasses_final.py:35:1: error[invalid-assignment] Cannot assign to final attribute `final_no_default` on type `D`
 dataclasses_final.py:36:1: error[invalid-assignment] Cannot assign to final attribute `final_with_default` on type `D`
@@ -263,7 +263,7 @@
 dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
 dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_match_args.py:42:1: error[unresolved-attribute] Class `DC4` has no attribute `__match_args__`
-dataclasses_match_args.py:49:1: error[type-assertion-failure] Argument does not have asserted type `tuple[()]`
+dataclasses_match_args.py:49:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `Unknown | tuple[()]`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
 dataclasses_postinit.py:28:7: error[unresolved-attribute] Object of type `DC1` has no attribute `x`
 dataclasses_postinit.py:29:7: error[unresolved-attribute] Object of type `DC1` has no attribute `y`
@@ -334,12 +334,12 @@
 dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_usage.py:130:1: error[missing-argument] No argument provided for required parameter `y` of bound method `__init__`
 dataclasses_usage.py:179:6: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
-directives_assert_type.py:27:5: error[type-assertion-failure] Argument does not have asserted type `int`
-directives_assert_type.py:28:5: error[type-assertion-failure] Argument does not have asserted type `Any`
-directives_assert_type.py:29:5: error[type-assertion-failure] Argument does not have asserted type `int`
-directives_assert_type.py:30:5: error[type-assertion-failure] Argument does not have asserted type `int`
+directives_assert_type.py:27:5: error[type-assertion-failure] Type `int` does not match asserted type `int | str`
+directives_assert_type.py:28:5: error[type-assertion-failure] Type `Any` does not match asserted type `int | str`
+directives_assert_type.py:29:5: error[type-assertion-failure] Type `int` does not match asserted type `Any`
+directives_assert_type.py:30:5: error[type-assertion-failure] Type `int` does not match asserted type `Literal[4]`
 directives_assert_type.py:32:5: error[missing-argument] No arguments provided for required parameters `value`, `type` of function `assert_type`
-directives_assert_type.py:33:5: error[type-assertion-failure] Argument does not have asserted type `int`
+directives_assert_type.py:33:5: error[type-assertion-failure] Type `int` does not match asserted type `Literal[""]`
 directives_assert_type.py:34:31: error[too-many-positional-arguments] Too many positional arguments to function `assert_type`: expected 2, got 3
 directives_cast.py:15:8: error[missing-argument] No arguments provided for required parameters `typ`, `val` of function `cast`
 directives_cast.py:16:13: error[invalid-type-form] Int literals are not allowed in this context in a type expression
@@ -370,86 +370,86 @@
 directives_version_platform.py:36:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:40:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:45:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
-enums_behaviors.py:27:1: error[type-assertion-failure] Argument does not have asserted type `Color`
-enums_behaviors.py:28:1: error[type-assertion-failure] Argument does not have asserted type `Literal[Color.RED]`
-enums_behaviors.py:32:1: error[type-assertion-failure] Argument does not have asserted type `Literal[Color.BLUE]`
+enums_behaviors.py:27:1: error[type-assertion-failure] Type `Color` does not match asserted type `Unknown`
+enums_behaviors.py:28:1: error[type-assertion-failure] Type `Literal[Color.RED]` does not match asserted type `Unknown`
+enums_behaviors.py:32:1: error[type-assertion-failure] Type `Literal[Color.BLUE]` does not match asserted type `Color`
 enums_behaviors.py:44:21: error[subclass-of-final-class] Class `ExtendedShape` cannot inherit from final class `Shape`
-enums_expansion.py:52:9: error[type-assertion-failure] Argument does not have asserted type `CustomFlags`
-enums_member_names.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE", "GREEN"]`
-enums_member_values.py:30:5: error[type-assertion-failure] Argument does not have asserted type `Literal[1, 2, 3]`
-enums_member_values.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
+enums_expansion.py:52:9: error[type-assertion-failure] Type `CustomFlags` does not match asserted type `Literal[CustomFlags.FLAG3]`
+enums_member_names.py:30:5: error[type-assertion-failure] Type `Literal["RED", "BLUE", "GREEN"]` does not match asserted type `Any`
+enums_member_values.py:30:5: error[type-assertion-failure] Type `Literal[1, 2, 3]` does not match asserted type `Any`
+enums_member_values.py:54:1: error[type-assertion-failure] Type `Literal[1]` does not match asserted type `tuple[Literal[1], float, float]`
 enums_member_values.py:85:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `_value_` of type `str`
-enums_member_values.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
-enums_members.py:82:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_member_values.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
+enums_members.py:82:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x) -> Unknown)`
 enums_members.py:82:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:83:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:83:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((...) -> Unknown)`
 enums_members.py:83:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:84:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:84:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `property`
 enums_members.py:84:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:85:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:85:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `def speak(self) -> None`
 enums_members.py:85:33: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:116:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:116:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | nonmember[int]`
 enums_members.py:116:32: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
-enums_members.py:129:9: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:129:9: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | Literal[2]`
 enums_members.py:129:43: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:146:1: error[type-assertion-failure] Argument does not have asserted type `int`
-enums_members.py:147:1: error[type-assertion-failure] Argument does not have asserted type `int`
-exceptions_context_managers.py:50:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-exceptions_context_managers.py:57:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
+enums_members.py:146:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown | Literal[2]`
+enums_members.py:147:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown | Literal[3]`
+exceptions_context_managers.py:50:5: error[type-assertion-failure] Type `int | str` does not match asserted type `str`
+exceptions_context_managers.py:57:5: error[type-assertion-failure] Type `int | str` does not match asserted type `str`
 generics_base_class.py:26:26: error[invalid-argument-type] Argument to function `takes_dict_incorrect` is incorrect: Expected `dict[str, list[object]]`, found `SymbolTable`
 generics_base_class.py:29:14: error[invalid-type-form] `typing.Generic` is not allowed in type expressions
 generics_base_class.py:30:8: error[invalid-type-form] `typing.Generic` is not allowed in type expressions
-generics_base_class.py:45:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[int]`
+generics_base_class.py:45:5: error[type-assertion-failure] Type `Iterator[int]` does not match asserted type `Unknown`
 generics_base_class.py:49:22: error[too-many-positional-arguments] Too many positional arguments to class `LinkedList`: expected 1, got 2
 generics_base_class.py:61:18: error[too-many-positional-arguments] Too many positional arguments to class `MyDict`: expected 1, got 2
 generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@concat` and `AnyStr@concat`
 generics_basic.py:49:44: error[invalid-legacy-type-variable] A `TypeVar` cannot have exactly one constraint
-generics_basic.py:139:5: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_basic.py:140:5: error[type-assertion-failure] Argument does not have asserted type `int`
+generics_basic.py:139:5: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
+generics_basic.py:140:5: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 generics_basic.py:157:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap1[str, int]`
 generics_basic.py:158:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap2[int, str].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap2[int, str]`
 generics_basic.py:162:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Generic`
 generics_basic.py:163:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Protocol`
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:172:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
-generics_basic.py:199:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[Any]`
-generics_defaults.py:30:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:31:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:32:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:38:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:46:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
+generics_basic.py:199:5: error[type-assertion-failure] Type `Iterator[Any]` does not match asserted type `Unknown`
+generics_defaults.py:30:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults'>`
+generics_defaults.py:31:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
+generics_defaults.py:32:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
+generics_defaults.py:38:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'OneDefault[int | float, bool]'>`
+generics_defaults.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults'>`
+generics_defaults.py:46:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:50:1: error[missing-argument] No argument provided for required parameter `T2` of class `AllTheDefaults`
-generics_defaults.py:52:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:55:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:59:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:63:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:79:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:80:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+generics_defaults.py:52:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:55:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:59:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:63:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:79:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Class_ParamSpec'>`
+generics_defaults.py:80:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:80:32: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
-generics_defaults.py:81:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+generics_defaults.py:81:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
 generics_defaults.py:81:46: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_defaults.py:94:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(specialized non-generic class)`
+generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Class_TypeVarTuple'>`
+generics_defaults.py:95:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `Class_TypeVarTuple`
 generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
-generics_defaults.py:131:1: error[type-assertion-failure] Argument does not have asserted type `Any`
+generics_defaults.py:131:1: error[type-assertion-failure] Type `Any` does not match asserted type `int`
 generics_defaults.py:142:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:152:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:155:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
 generics_defaults.py:156:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
-generics_defaults.py:170:1: error[type-assertion-failure] Argument does not have asserted type `(Foo7[int], /) -> Foo7[int]`
-generics_defaults_referential.py:23:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
+generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth(self, /) -> Self@meth`
+generics_defaults_referential.py:23:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'slice'>`
 generics_defaults_referential.py:36:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
-generics_defaults_referential.py:94:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults_referential.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
-generics_defaults_specialization.py:26:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, str]`
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Argument does not have asserted type `SomethingWithNoDefaults[int, bool]`
+generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
+generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar[int, list[int]]'>`
+generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
+generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(unknown type subscript)`
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
-generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
+generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
 generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -457,7 +457,7 @@
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
 generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
 generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:22:1: error[type-assertion-failure] Argument does not have asserted type `(str, bool, /) -> str`
+generics_paramspec_semantics.py:22:1: error[type-assertion-failure] Type `(str, bool, /) -> str` does not match asserted type `(...) -> str`
 generics_paramspec_semantics.py:30:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
 generics_paramspec_semantics.py:34:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:38:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -465,7 +465,7 @@
 generics_paramspec_semantics.py:57:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:76:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 generics_paramspec_semantics.py:82:28: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
-generics_paramspec_semantics.py:84:5: error[type-assertion-failure] Argument does not have asserted type `(int, /) -> str`
+generics_paramspec_semantics.py:84:5: error[type-assertion-failure] Type `(int, /) -> str` does not match asserted type `(...) -> str`
 generics_paramspec_semantics.py:87:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:91:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
 generics_paramspec_semantics.py:101:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
@@ -479,27 +479,27 @@
 generics_paramspec_specialization.py:40:31: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_paramspec_specialization.py:52:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str, bool]`?
 generics_paramspec_specialization.py:58:15: error[too-many-positional-arguments] Too many positional arguments to class `ClassC`: expected 1, got 3
-generics_scoping.py:14:1: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
-generics_scoping.py:42:1: error[type-assertion-failure] Argument does not have asserted type `str`
-generics_scoping.py:43:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
+generics_scoping.py:14:1: error[type-assertion-failure] Type `int` does not match asserted type `Literal[1]`
+generics_scoping.py:15:1: error[type-assertion-failure] Type `str` does not match asserted type `Literal["a"]`
+generics_scoping.py:42:1: error[type-assertion-failure] Type `str` does not match asserted type `Literal["abc"]`
+generics_scoping.py:43:1: error[type-assertion-failure] Type `bytes` does not match asserted type `Literal[b"abc"]`
 generics_scoping.py:65:11: error[invalid-generic-class] Generic class `MyGeneric` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_scoping.py:75:11: error[invalid-generic-class] Generic class `Bad` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@prop1`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method2]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method3]`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self@method3`
-generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self@method3`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `list[typing.Self]`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `typing.Self`
+generics_self_advanced.py:38:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `Unknown`
+generics_self_advanced.py:43:9: error[type-assertion-failure] Type `list[Self@method3]` does not match asserted type `Unknown`
+generics_self_advanced.py:44:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
+generics_self_advanced.py:45:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
-generics_self_basic.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
-generics_self_basic.py:55:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
+generics_self_basic.py:54:1: error[type-assertion-failure] Type `Shape` does not match asserted type `Unknown`
+generics_self_basic.py:55:1: error[type-assertion-failure] Type `Circle` does not match asserted type `Unknown`
 generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@set_value`
 generics_self_basic.py:67:26: error[invalid-type-form] Special form `typing.Self` expected no type parameter
 generics_self_protocols.py:61:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `BadReturnType`
@@ -560,17 +560,17 @@
 generics_syntax_scoping.py:40:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R@decorator1, /) -> (...) -> R@decorator1`
 generics_syntax_scoping.py:44:17: error[unresolved-reference] Name `T` used when not defined
 generics_syntax_scoping.py:81:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R@decorator2, /) -> (...) -> R@decorator2`
-generics_syntax_scoping.py:113:9: error[type-assertion-failure] Argument does not have asserted type `str`
-generics_syntax_scoping.py:116:13: error[type-assertion-failure] Argument does not have asserted type `TypeVar`
-generics_syntax_scoping.py:121:9: error[type-assertion-failure] Argument does not have asserted type `int | float | complex`
-generics_syntax_scoping.py:124:13: error[type-assertion-failure] Argument does not have asserted type `int | float | complex`
+generics_syntax_scoping.py:113:9: error[type-assertion-failure] Type `str` does not match asserted type `Literal[""]`
+generics_syntax_scoping.py:116:13: error[type-assertion-failure] Type `TypeVar` does not match asserted type `typing.TypeVar`
+generics_syntax_scoping.py:121:9: error[type-assertion-failure] Type `int | float | complex` does not match asserted type `complex`
+generics_syntax_scoping.py:124:13: error[type-assertion-failure] Type `int | float | complex` does not match asserted type `complex`
 generics_type_erasure.py:38:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Literal[""]`
 generics_type_erasure.py:40:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Literal[0]`
 generics_typevartuple_args.py:16:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_args.py:20:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str]`
+generics_typevartuple_args.py:20:1: error[type-assertion-failure] Type `tuple[int, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_args.py:27:77: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_args.py:31:1: error[type-assertion-failure] Argument does not have asserted type `tuple[()]`
-generics_typevartuple_args.py:32:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str]`
+generics_typevartuple_args.py:31:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_args.py:32:1: error[type-assertion-failure] Type `tuple[int, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
@@ -579,45 +579,45 @@
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:75:50: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int]`
+generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:106:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_callable.py:29:57: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:33:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[int | float | complex, str, int]`
 generics_typevartuple_callable.py:37:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str]`
-generics_typevartuple_callable.py:41:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int, int | float | complex]`
-generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str]`
+generics_typevartuple_callable.py:41:1: error[type-assertion-failure] Type `tuple[str, int, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Type `tuple[str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:45:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float, str, int | float | complex]`
+generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Type `tuple[int | float, str, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:22:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, bool, str]`
+generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int | float, bool]`
-generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, @Todo(specialized non-generic class)]`
+generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(unknown type subscript)`
 generics_typevartuple_specialization.py:50:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:50:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int]`
-generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, @Todo(specialized non-generic class)]`
+generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(unknown type subscript)`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
-generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float]`
-generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Argument does not have asserted type `tuple[Any, *tuple[Any, ...]]`
+generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
-generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool]`
-generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str], bool, int | float]`
-generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str, bool], int | float, int]`
+generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
+generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Type `tuple[tuple[str], bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
+generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Type `tuple[tuple[str, bool], int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func9, T2@func9, T3@func9]`
-generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool, int | float]`
-generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[bool], str, int | float, int]`
-generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Argument does not have asserted type `tuple[*tuple[int, ...], int]`
-generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Argument does not have asserted type `tuple[*tuple[int, ...], str]`
-generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Argument does not have asserted type `tuple[*tuple[int, ...], str]`
+generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
+generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
+generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `@Todo(Support for `typing.GenericAlias` instances in type expressions)`
+generics_typevartuple_specialization.py

... (truncated 268 lines) ...
```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-17 09:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
asynq (https://github.com/quora/asynq)
- asynq/tests/test_typing.py:16:9: error[type-assertion-failure] Argument does not have asserted type `FutureBase[str]`
+ asynq/tests/test_typing.py:16:9: error[type-assertion-failure] Type `FutureBase[str]` does not match asserted type `FutureBase[Unknown]`

pytest (https://github.com/pytest-dev/pytest)
- testing/typing_checks.py:54:5: error[type-assertion-failure] Argument does not have asserted type `ExceptionInfo[RuntimeError] | None`
+ testing/typing_checks.py:54:5: error[type-assertion-failure] Type `ExceptionInfo[RuntimeError] | None` does not match asserted type `ExceptionInfo[BaseException] | None`
- testing/typing_checks.py:59:5: error[type-assertion-failure] Argument does not have asserted type `Literal["setup", "call", "teardown"]`
+ testing/typing_checks.py:59:5: error[type-assertion-failure] Type `Literal["setup", "call", "teardown"]` does not match asserted type `str | None`
- testing/typing_checks.py:60:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int | None, str]`
+ testing/typing_checks.py:60:5: error[type-assertion-failure] Type `tuple[str, int | None, str]` does not match asserted type `tuple[str, int | None, str] | None`
- testing/typing_raises_group.py:237:5: error[type-assertion-failure] Argument does not have asserted type `((...) -> bool) | None`
+ testing/typing_raises_group.py:237:5: error[type-assertion-failure] Type `((...) -> bool) | None` does not match asserted type `Unknown | ((@Todo, /) -> bool) | None`

starlette (https://github.com/encode/starlette)
- tests/test_config.py:20:5: error[type-assertion-failure] Argument does not have asserted type `str`
+ tests/test_config.py:20:5: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
- tests/test_config.py:21:5: error[type-assertion-failure] Argument does not have asserted type `str | None`
+ tests/test_config.py:21:5: error[type-assertion-failure] Type `str | None` does not match asserted type `Unknown`
- tests/test_config.py:22:5: error[type-assertion-failure] Argument does not have asserted type `str | None`
+ tests/test_config.py:22:5: error[type-assertion-failure] Type `str | None` does not match asserted type `None`
- tests/test_config.py:23:5: error[type-assertion-failure] Argument does not have asserted type `str`
+ tests/test_config.py:23:5: error[type-assertion-failure] Type `str` does not match asserted type `Literal[""]`
- tests/test_config.py:25:5: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_config.py:25:5: error[type-assertion-failure] Type `bool` does not match asserted type `Unknown`
+ tests/test_config.py:26:5: error[type-assertion-failure] Type `bool` does not match asserted type `Literal[False]`
- tests/test_config.py:26:5: error[type-assertion-failure] Argument does not have asserted type `bool`
+ tests/test_config.py:27:5: error[type-assertion-failure] Type `bool | None` does not match asserted type `None`
- tests/test_config.py:27:5: error[type-assertion-failure] Argument does not have asserted type `bool | None`

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
- test/test_generated_mypy.py:492:5: error[type-assertion-failure] Argument does not have asserted type `UserId`
+ test/test_generated_mypy.py:492:5: error[type-assertion-failure] Type `UserId` does not match asserted type `Unknown`
- test/test_generated_mypy.py:493:5: error[type-assertion-failure] Argument does not have asserted type `Email`
+ test/test_generated_mypy.py:493:5: error[type-assertion-failure] Type `Email` does not match asserted type `Unknown`
- test/test_generated_mypy.py:494:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[UserId, Email]`
+ test/test_generated_mypy.py:494:5: error[type-assertion-failure] Type `ScalarMap[UserId, Email]` does not match asserted type `Unknown`
- test/test_generated_mypy.py:503:5: error[type-assertion-failure] Argument does not have asserted type `UserId`
+ test/test_generated_mypy.py:503:5: error[type-assertion-failure] Type `UserId` does not match asserted type `Unknown`
- test/test_generated_mypy.py:504:5: error[type-assertion-failure] Argument does not have asserted type `Email`
+ test/test_generated_mypy.py:504:5: error[type-assertion-failure] Type `Email` does not match asserted type `Unknown`
- test/test_generated_mypy.py:505:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[UserId, Email]`
+ test/test_generated_mypy.py:505:5: error[type-assertion-failure] Type `ScalarMap[UserId, Email]` does not match asserted type `Unknown`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_dns_resolver.py:155:17: error[type-assertion-failure] Argument does not have asserted type `Never`
+ test/mitmproxy/addons/test_dns_resolver.py:155:17: error[type-assertion-failure] Type `@Todo` is not equivalent to `Never`

artigraph (https://github.com/artigraph/artigraph)
- src/arti/internal/mappings.py:231:17: error[type-assertion-failure] Argument does not have asserted type `Never`
+ src/arti/internal/mappings.py:231:17: error[type-assertion-failure] Type `Unknown & ~Literal["closed"] & ~Literal["open"]` is not equivalent to `Never`

psycopg (https://github.com/psycopg/psycopg)
- tests/pool/test_pool.py:72:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[MyRow]`
+ tests/pool/test_pool.py:72:5: error[type-assertion-failure] Type `MyConnection[MyRow]` does not match asserted type `Unknown`
- tests/pool/test_pool.py:73:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool.py:73:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool.py:82:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[@Todo]`
+ tests/pool/test_pool.py:82:5: error[type-assertion-failure] Type `MyConnection[@Todo]` does not match asserted type `Unknown`
- tests/pool/test_pool.py:105:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection`
+ tests/pool/test_pool.py:105:5: error[type-assertion-failure] Type `MyConnection` does not match asserted type `Unknown`
- tests/pool/test_pool.py:106:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool.py:106:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool_async.py:70:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[MyRow]`
+ tests/pool/test_pool_async.py:70:5: error[type-assertion-failure] Type `MyConnection[MyRow]` does not match asserted type `Unknown`
- tests/pool/test_pool_async.py:71:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool_async.py:71:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool_async.py:82:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[@Todo]`
+ tests/pool/test_pool_async.py:82:5: error[type-assertion-failure] Type `MyConnection[@Todo]` does not match asserted type `Unknown`
- tests/pool/test_pool_async.py:105:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection`
+ tests/pool/test_pool_async.py:105:5: error[type-assertion-failure] Type `MyConnection` does not match asserted type `Unknown`
- tests/pool/test_pool_async.py:106:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool_async.py:106:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool_null.py:66:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[MyRow]`
+ tests/pool/test_pool_null.py:66:5: error[type-assertion-failure] Type `MyConnection[MyRow]` does not match asserted type `Unknown`
- tests/pool/test_pool_null.py:67:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool_null.py:67:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool_null.py:76:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[@Todo]`
+ tests/pool/test_pool_null.py:76:5: error[type-assertion-failure] Type `MyConnection[@Todo]` does not match asserted type `Unknown`
- tests/pool/test_pool_null.py:98:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection`
+ tests/pool/test_pool_null.py:98:5: error[type-assertion-failure] Type `MyConnection` does not match asserted type `Unknown`
- tests/pool/test_pool_null.py:99:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool_null.py:99:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool_null_async.py:65:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[MyRow]`
+ tests/pool/test_pool_null_async.py:65:5: error[type-assertion-failure] Type `MyConnection[MyRow]` does not match asserted type `Unknown`
- tests/pool/test_pool_null_async.py:66:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool_null_async.py:66:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`
- tests/pool/test_pool_null_async.py:77:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection[@Todo]`
+ tests/pool/test_pool_null_async.py:77:5: error[type-assertion-failure] Type `MyConnection[@Todo]` does not match asserted type `Unknown`
- tests/pool/test_pool_null_async.py:97:5: error[type-assertion-failure] Argument does not have asserted type `MyConnection`
+ tests/pool/test_pool_null_async.py:97:5: error[type-assertion-failure] Type `MyConnection` does not match asserted type `Unknown`
- tests/pool/test_pool_null_async.py:98:5: error[type-assertion-failure] Argument does not have asserted type `MyRow`
+ tests/pool/test_pool_null_async.py:98:5: error[type-assertion-failure] Type `MyRow` does not match asserted type `Unknown`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Unknown key "pair" for TypedDict `RPCStatusMsg`: Unknown key "pair"
- freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Unknown key "pair" for TypedDict `RPCStrategyMsg`: Unknown key "pair"
- freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Unknown key "pair" for TypedDict `RPCWhitelistMsg` - did you mean "data"?
- freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Unknown key "pair" for TypedDict `RPCAnalyzedDFMsg` - did you mean "data"?
- freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Unknown key "pair" for TypedDict `RPCNewCandleMsg` - did you mean "data"?
- freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Unknown key "trade_id" for TypedDict `RPCStatusMsg`: Unknown key "trade_id"
- freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Unknown key "trade_id" for TypedDict `RPCStrategyMsg`: Unknown key "trade_id"
- freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Unknown key "trade_id" for TypedDict `RPCProtectionMsg`: Unknown key "trade_id"
- freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Unknown key "trade_id" for TypedDict `RPCWhitelistMsg`: Unknown key "trade_id"
- freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Unknown key "trade_id" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "trade_id"
- freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Unknown key "trade_id" for TypedDict `RPCNewCandleMsg`: Unknown key "trade_id"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCExitMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCStatusMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCEntryMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCStrategyMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCStatusMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCStrategyMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCEntryMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCExitMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Unknown key "pair" for TypedDict `RPCStatusMsg`: Unknown key "pair"
- freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Unknown key "pair" for TypedDict `RPCStrategyMsg`: Unknown key "pair"
- freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Unknown key "pair" for TypedDict `RPCWhitelistMsg` - did you mean "data"?
- freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Unknown key "pair" for TypedDict `RPCAnalyzedDFMsg` - did you mean "data"?
- freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Unknown key "pair" for TypedDict `RPCNewCandleMsg` - did you mean "data"?
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCStatusMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCEntryMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCExitMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCNewCandleMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCStrategyMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCWhitelistMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCStatusMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCStrategyMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCEntryMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCExitMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Unknown key "reason" for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCWhitelistMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCEntryMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCExitMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCNewCandleMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCStatusMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCStrategyMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCStrategyMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCProtectionMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCWhitelistMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCEntryMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCExitMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Unknown key "status" for TypedDict `RPCNewCandleMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCStrategyMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCProtectionMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCWhitelistMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCEntryMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCExitMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Unknown key "status" for TypedDict `RPCNewCandleMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCEntryMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCStrategyMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCExitMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCNewCandleMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCWhitelistMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Unknown key "status" for TypedDict `RPCProtectionMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCStrategyMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCProtectionMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCWhitelistMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCEntryMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCExitMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Unknown key "status" for TypedDict `RPCNewCandleMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCStatusMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCWhitelistMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCEntryMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCCancelMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCExitMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCExitCancelMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCNewCandleMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "msg" - did you mean "id"?
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCProtectionMsg` - did you mean "id"?
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCStatusMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCStrategyMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCProtectionMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCWhitelistMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCEntryMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCCancelMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCAnalyzedDFMsg`: Unknown key "exit_reason"
- freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Unknown key "exit_reason" for TypedDict `RPCNewCandleMsg`: Unknown key "exit_reason"

discord.py (https://github.com/Rapptz/discord.py)
- discord/automod.py:164:49: error[invalid-key] Invalid key for TypedDict `_AutoModerationActionMetadataCustomMessage`: Unknown key "duration_seconds"
+ discord/automod.py:164:49: error[invalid-key] Unknown key "duration_seconds" for TypedDict `_AutoModerationActionMetadataCustomMessage`: Unknown key "duration_seconds"
- discord/automod.py:164:49: error[invalid-key] Invalid key for TypedDict `_AutoModerationActionMetadataAlert`: Unknown key "duration_seconds"
+ discord/automod.py:164:49: error[invalid-key] Unknown key "duration_seconds" for TypedDict `_AutoModerationActionMetadataAlert`: Unknown key "duration_seconds"
- discord/automod.py:167:47: error[invalid-key] Invalid key for TypedDict `_AutoModerationActionMetadataCustomMessage`: Unknown key "channel_id"
+ discord/automod.py:167:47: error[invalid-key] Unknown key "channel_id" for TypedDict `_AutoModerationActionMetadataCustomMessage`: Unknown key "channel_id"
- discord/automod.py:167:47: error[invalid-key] Invalid key for TypedDict `_AutoModerationActionMetadataTimeout`: Unknown key "channel_id"
+ discord/automod.py:167:47: error[invalid-key] Unknown key "channel_id" for TypedDict `_AutoModerationActionMetadataTimeout`: Unknown key "channel_id"
- discord/state.py:822:31: error[invalid-key] Invalid key for TypedDict `PingInteraction`: Unknown key "data"
+ discord/state.py:822:31: error[invalid-key] Unknown key "data" for TypedDict `PingInteraction`: Unknown key "data"
- discord/state.py:823:36: error[invalid-key] Invalid key for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "custom_id"
+ discord/state.py:823:36: error[invalid-key] Unknown key "custom_id" for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "custom_id"
- discord/state.py:823:36: error[invalid-key] Invalid key for TypedDict `UserApplicationCommandInteractionData`: Unknown key "custom_id"
+ discord/state.py:823:36: error[invalid-key] Unknown key "custom_id" for TypedDict `UserApplicationCommandInteractionData`: Unknown key "custom_id"
- discord/state.py:823:36: error[invalid-key] Invalid key for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "custom_id"
+ discord/state.py:823:36: error[invalid-key] Unknown key "custom_id" for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "custom_id"
- discord/state.py:824:41: error[invalid-key] Invalid key for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "component_type"
+ discord/state.py:824:41: error[invalid-key] Unknown key "component_type" for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "component_type"
- discord/state.py:824:41: error[invalid-key] Invalid key for TypedDict `UserApplicationCommandInteractionData`: Unknown key "component_type"
+ discord/state.py:824:41: error[invalid-key] Unknown key "component_type" for TypedDict `UserApplicationCommandInteractionData`: Unknown key "component_type"
- discord/state.py:824:41: error[invalid-key] Invalid key for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "component_type"
+ discord/state.py:824:41: error[invalid-key] Unknown key "component_type" for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "component_type"
- discord/state.py:824:41: error[invalid-key] Invalid key for TypedDict `ModalSubmitInteractionData`: Unknown key "component_type"
+ discord/state.py:824:41: error[invalid-key] Unknown key "component_type" for TypedDict `ModalSubmitInteractionData`: Unknown key "component_type"
- discord/state.py:828:31: error[invalid-key] Invalid key for TypedDict `PingInteraction`: Unknown key "data"
+ discord/state.py:828:31: error[invalid-key] Unknown key "data" for TypedDict `PingInteraction`: Unknown key "data"
- discord/state.py:829:36: error[invalid-key] Invalid key for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "custom_id"
+ discord/state.py:829:36: error[invalid-key] Unknown key "custom_id" for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "custom_id"
- discord/state.py:829:36: error[invalid-key] Invalid key for TypedDict `UserApplicationCommandInteractionData`: Unknown key "custom_id"
+ discord/state.py:829:36: error[invalid-key] Unknown key "custom_id" for TypedDict `UserApplicationCommandInteractionData`: Unknown key "custom_id"
- discord/state.py:829:36: error[invalid-key] Invalid key for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "custom_id"
+ discord/state.py:829:36: error[invalid-key] Unknown key "custom_id" for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "custom_id"
- discord/state.py:830:37: error[invalid-key] Invalid key for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "components"
+ discord/state.py:830:37: error[invalid-key] Unknown key "components" for TypedDict `ChatInputApplicationCommandInteractionData`: Unknown key "components"
- discord/state.py:830:37: error[invalid-key] Invalid key for TypedDict `UserApplicationCommandInteractionData`: Unknown key "components"
+ discord/state.py:830:37: error[invalid-key] Unknown key "components" for TypedDict `UserApplicationCommandInteractionData`: Unknown key "components"
- discord/state.py:830:37: error[invalid-key] Invalid key for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "components"
+ discord/state.py:830:37: error[invalid-key] Unknown key "components" for TypedDict `MessageApplicationCommandInteractionData`: Unknown key "components"
- discord/state.py:830:37: error[invalid-key] Invalid key for TypedDict `ButtonMessageComponentInteractionData`: Unknown key "components"
+ discord/state.py:830:37: error[invalid-key] Unknown key "components" for TypedDict `ButtonMessageComponentInteractionData`: Unknown key "components"
- discord/state.py:830:37: error[invalid-key] Invalid key for TypedDict `SelectMessageComponentInteractionData`: Unknown key "components"
+ discord/state.py:830:37: error[invalid-key] Unknown key "components" for TypedDict `SelectMessageComponentInteractionData`: Unknown key "components"

cloud-init (https://github.com/canonical/cloud-init)
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:24:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "name"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:24:5: error[invalid-key] Unknown key "name" for TypedDict `MetaSchema`: Unknown key "name"
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:25:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "title"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:25:5: error[invalid-key] Unknown key "title" for TypedDict `MetaSchema`: Unknown key "title"
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:26:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "description"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:26:5: error[invalid-key] Unknown key "description" for TypedDict `MetaSchema`: Unknown key "description"
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:30:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "examples"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:30:5: error[invalid-key] Unknown key "examples" for TypedDict `MetaSchema`: Unknown key "examples"
- tests/unittests/config/test_modules.py:73:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "name"
+ tests/unittests/config/test_modules.py:73:13: error[invalid-key] Unknown key "name" for TypedDict `MetaSchema`: Unknown key "name"
- tests/unittests/config/test_modules.py:75:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "title"
+ tests/unittests/config/test_modules.py:75:13: error[invalid-key] Unknown key "title" for TypedDict `MetaSchema`: Unknown key "title"
- tests/unittests/config/test_modules.py:76:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "description"
+ tests/unittests/config/test_modules.py:76:13: error[invalid-key] Unknown key "description" for TypedDict `MetaSchema`: Unknown key "description"
- tests/unittests/config/test_modules.py:78:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "examples"
+ tests/unittests/config/test_modules.py:78:13: error[invalid-key] Unknown key "examples" for TypedDict `MetaSchema`: Unknown key "examples"
- tests/unittests/config/test_modules.py:113:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "name"
+ tests/unittests/config/test_modules.py:113:13: error[invalid-key] Unknown key "name" for TypedDict `MetaSchema`: Unknown key "name"
- tests/unittests/config/test_modules.py:115:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "title"
+ tests/unittests/config/test_modules.py:115:13: error[invalid-key] Unknown key "title" for TypedDict `MetaSchema`: Unknown key "title"
- tests/unittests/config/test_modules.py:116:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "description"
+ tests/unittests/config/test_modules.py:116:13: error[invalid-key] Unknown key "description" for TypedDict `MetaSchema`: Unknown key "description"
- tests/unittests/config/test_modules.py:118:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "examples"
+ tests/unittests/config/test_modules.py:118:13: error[invalid-key] Unknown key "examples" for TypedDict `MetaSchema`: Unknown key "examples"

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cargo/manifest.py:304:17: error[invalid-key] Invalid key for TypedDict `FromWorkspace`: Unknown key "version"
+ mesonbuild/cargo/manifest.py:304:17: error[invalid-key] Unknown key "version" for TypedDict `FromWorkspace`: Unknown key "version"
- mesonbuild/interpreter/interpreter.py:1759:98: error[invalid-key] Invalid key for TypedDict `FindProgram`: Unknown key "version_argument"
+ mesonbuild/interpreter/interpreter.py:1759:98: error[invalid-key] Unknown key "version_argument" for TypedDict `FindProgram`: Unknown key "version_argument"
- mesonbuild/interpreter/interpreter.py:2272:29: error[invalid-key] Invalid key for TypedDict `BaseTest`: Unknown key "args"
+ mesonbuild/interpreter/interpreter.py:2272:29: error[invalid-key] Unknown key "args" for TypedDict `BaseTest`: Unknown key "args"
- mesonbuild/interpreter/interpreter.py:2277:29: error[invalid-key] Invalid key for TypedDict `BaseTest`: Unknown key "protocol"
+ mesonbuild/interpreter/interpreter.py:2277:29: error[invalid-key] Unknown key "protocol" for TypedDict `BaseTest`: Unknown key "protocol"
- mesonbuild/interpreter/interpreter.py:2279:29: error[invalid-key] Invalid key for TypedDict `BaseTest`: Unknown key "verbose"
+ mesonbuild/interpreter/interpreter.py:2279:29: error[invalid-key] Unknown key "verbose" for TypedDict `BaseTest`: Unknown key "verbose"
- mesonbuild/interpreter/interpreter.py:2322:19: error[invalid-key] Invalid key for TypedDict `FuncInstallHeaders`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2322:19: error[invalid-key] Unknown key "preserve_path" for TypedDict `FuncInstallHeaders`: Unknown key "preserve_path"
- mesonbuild/interpreter/interpreter.py:2512:23: error[invalid-key] Invalid key for TypedDict `FuncInstallData`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2512:23: error[invalid-key] Unknown key "preserve_path" for TypedDict `FuncInstallData`: Unknown key "preserve_path"
- mesonbuild/interpreter/interpreter.py:2518:90: error[invalid-key] Invalid key for TypedDict `FuncInstallData`: Unknown key "install_tag" - did you mean "install_dir"?
+ mesonbuild/interpreter/interpreter.py:2518:90: error[invalid-key] Unknown key "install_tag" for TypedDict `FuncInstallData` - did you mean "install_dir"?
- mesonbuild/interpreter/interpreter.py:2519:60: error[invalid-key] Invalid key for TypedDict `FuncInstallData`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2519:60: error[invalid-key] Unknown key "preserve_path" for TypedDict `FuncInstallData`: Unknown key "preserve_path"
- mesonbuild/interpreter/interpreter.py:2586:32: error[

... (truncated 17277 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 09:41_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-17 10:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 11:02_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 0 | 0 | 4,853 |
| `invalid-key` | 0 | 0 | 3,943 |
| **Total** | **0** | **0** | **8,796** |

**[Full report with detailed diff](https://david-custom-concise-diagnos.ecosystem-663.pages.dev/diff)** ([timing results](https://david-custom-concise-diagnos.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @sharkdp on 2025-11-17 11:04_

---

_Review requested from @carljm by @sharkdp on 2025-11-17 11:04_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-17 11:04_

---

_Review requested from @dcreager by @sharkdp on 2025-11-17 11:04_

---

_Review requested from @MichaReiser by @sharkdp on 2025-11-17 11:04_

---

_Review requested from @BurntSushi by @sharkdp on 2025-11-17 11:05_

---

_Comment by @sharkdp on 2025-11-17 12:25_

mypy_primer is timing out, probably because the diff is too large. Ecosystem analyzer finishes just fine.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Unknown_key_for_all_…_(1c685d9d10678263).snap`:51 on 2025-11-17 12:56_

I don't love that with the default output format, the phrase `Unknown key "surname"` is now repeated in both the diagnostic summary and the primary annotation. What about something like this?

<details>
<summary>Patch</summary>

```diff
diff --git "a/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Misspelled_key_for_`\342\200\246_(7cf0fa634e2a2d59).snap" "b/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Misspelled_key_for_`\342\200\246_(7cf0fa634e2a2d59).snap"
index b65345fd37..ca95ce1675 100644
--- "a/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Misspelled_key_for_`\342\200\246_(7cf0fa634e2a2d59).snap"
+++ "b/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Misspelled_key_for_`\342\200\246_(7cf0fa634e2a2d59).snap"
@@ -29,7 +29,7 @@ error[invalid-key]: Unknown key "Retries" for TypedDict `Config`
   |
 6 | def _(config: Config) -> None:
 7 |     config["Retries"] = 30.0  # error: [invalid-key]
-  |     ------ ^^^^^^^^^ Unknown key "Retries" - did you mean "retries"?
+  |     ------ ^^^^^^^^^ Did you mean "retries"?
   |     |
   |     TypedDict `Config`
   |
diff --git "a/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Unknown_key_for_all_\342\200\246_(1c685d9d10678263).snap" "b/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Unknown_key_for_all_\342\200\246_(1c685d9d10678263).snap"
index 6e7a0eba6a..6c9e4de308 100644
--- "a/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Unknown_key_for_all_\342\200\246_(1c685d9d10678263).snap"
+++ "b/crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti\342\200\246_-_Subscript_assignment\342\200\246_-_Unknown_key_for_all_\342\200\246_(1c685d9d10678263).snap"
@@ -36,7 +36,7 @@ error[invalid-key]: Unknown key "surname" for TypedDict `Person`
 11 |     # error: [invalid-key]
 12 |     # error: [invalid-key]
 13 |     being["surname"] = "unknown"
-   |     ----- ^^^^^^^^^ Unknown key "surname" - did you mean "name"?
+   |     ----- ^^^^^^^^^ Did you mean "name"?
    |     |
    |     TypedDict `Person` in union type `Person | Animal`
    |
@@ -51,7 +51,7 @@ error[invalid-key]: Unknown key "surname" for TypedDict `Animal`
 11 |     # error: [invalid-key]
 12 |     # error: [invalid-key]
 13 |     being["surname"] = "unknown"
-   |     ----- ^^^^^^^^^ Unknown key "surname" - did you mean "name"?
+   |     ----- ^^^^^^^^^ Did you mean "name"?
    |     |
    |     TypedDict `Animal` in union type `Person | Animal`
    |
diff --git a/crates/ty_python_semantic/resources/mdtest/snapshots/typed_dict.md_-_`TypedDict`_-_Diagnostics_(e5289abf5c570c29).snap b/crates/ty_python_semantic/resources/mdtest/snapshots/typed_dict.md_-_`TypedDict`_-_Diagnostics_(e5289abf5c570c29).snap
index e2e6ac1bdf..cdd2949031 100644
--- a/crates/ty_python_semantic/resources/mdtest/snapshots/typed_dict.md_-_`TypedDict`_-_Diagnostics_(e5289abf5c570c29).snap
+++ b/crates/ty_python_semantic/resources/mdtest/snapshots/typed_dict.md_-_`TypedDict`_-_Diagnostics_(e5289abf5c570c29).snap
@@ -62,7 +62,7 @@ error[invalid-key]: Unknown key "naem" for TypedDict `Person`
    |
  7 | def access_invalid_literal_string_key(person: Person):
  8 |     person["naem"]  # error: [invalid-key]
-   |     ------ ^^^^^^ Unknown key "naem" - did you mean "name"?
+   |     ------ ^^^^^^ Did you mean "name"?
    |     |
    |     TypedDict `Person`
  9 |
@@ -78,7 +78,7 @@ error[invalid-key]: Unknown key "naem" for TypedDict `Person`
    |
 12 | def access_invalid_key(person: Person):
 13 |     person[NAME_KEY]  # error: [invalid-key]
-   |     ------ ^^^^^^^^ Unknown key "naem" - did you mean "name"?
+   |     ------ ^^^^^^^^ Did you mean "name"?
    |     |
    |     TypedDict `Person`
 14 |
@@ -135,7 +135,7 @@ error[invalid-key]: Unknown key "naem" for TypedDict `Person`
    |
 21 | def write_to_non_existing_key(person: Person):
 22 |     person["naem"] = "Alice"  # error: [invalid-key]
-   |     ------ ^^^^^^ Unknown key "naem" - did you mean "name"?
+   |     ------ ^^^^^^ Did you mean "name"?
    |     |
    |     TypedDict `Person`
 23 |
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index 68f42ea1a4..810a45f658 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -3167,16 +3167,14 @@ pub(crate) fn report_invalid_key_on_typed_dict<'db>(
                 });
 
                 let existing_keys = items.iter().map(|(name, _)| name.as_str());
-                let hint = if let Some(suggestion) = did_you_mean(existing_keys, key) {
-                    format!(" - did you mean \"{suggestion}\"?")
+                if let Some(suggestion) = did_you_mean(existing_keys, key) {
+                    diagnostic.set_primary_message(format_args!("Did you mean \"{suggestion}\"?"));
+                    diagnostic.set_concise_message(format_args!(
+                        "Unknown key \"{key}\" for TypedDict `{typed_dict_name}` - did you mean \"{suggestion}\"?",
+                    ));
                 } else {
-                    String::new()
-                };
-
-                diagnostic.set_primary_message(format!("Unknown key \"{key}\"{hint}"));
-                diagnostic.set_concise_message(format!(
-                    "Unknown key \"{key}\" for TypedDict `{typed_dict_name}`{hint}",
-                ));
+                    diagnostic.set_primary_message(format_args!("Unknown key \"{key}\""));
+                }
             }
             _ => {
                 let mut diagnostic = builder.into_diagnostic(format_args!(
```

</details>

---

_@AlexWaygood approved on 2025-11-17 12:57_

Thank you. I agree that this is something I've wanted (and worked around!) on multiple PRs, and that it's something that will benefit our users a lot.

---

_@sharkdp reviewed on 2025-11-17 13:04_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Unknown_key_for_all_…_(1c685d9d10678263).snap`:51 on 2025-11-17 13:04_

Hm, that might be slightly weird in cases where the key is not a literal string expression, right? Like if I use `MY_KEY: Final = "my_key"`, and then access the typed dict with `my_typed_dict[MY_KEY]`. It then looks like we literally want you to replace `MY_KEY` with `"correctly_spelled_key"`.

---

_@AlexWaygood reviewed on 2025-11-17 13:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Unknown_key_for_all_…_(1c685d9d10678263).snap`:51 on 2025-11-17 13:11_

Right, fair point. You could add another check to see whether it was an `ExprStringLiteral` node that was used to subscript the `TypedDict` (which I'd expect to be the common case)

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Unknown_key_for_all_…_(1c685d9d10678263).snap`:51 on 2025-11-17 13:25_

You might also consider just setting the primary annotation to "unknown key" and adding the "did you mean" as a note sub-diagnostic. I guess that might still be a little weird with the `MY_KEY` example, but maybe that weirdness is best addressed by @AlexWaygood's suggestion.

---

_@BurntSushi approved on 2025-11-17 13:31_

I remain skeptical of complicating our diagnostic model in order to improve concise diagnostics. I realize this is framed as also improving our verbose diagnostics, but I would consider relegating concise diagnostics to "best effort" only and improve the verbose diagnostic while mostly ignoring the quality of the concise output. But this is predicated on the assumption that the quality of the concise output isn't That Important, and perhaps my instincts are wrong there. I'm happy to trust you and @AlexWaygood's judgment that they actually are important and that it _is_ worth complicating our diagnostic model.

 I do like the light touch here and that it's opt-in. Thank you for putting this together. :-)

---

_Comment by @AlexWaygood on 2025-11-17 13:48_

Like @sharkdp, I have on multiple occasions at this point deliberately created a worse verbose diagnostic for the sake of creating a concise diagnostic that would be usable. So I do agree that the current situation is hurting all our output formats, not just concise output formats

> this is predicated on the assumption that the quality of the concise output isn't That Important,

I have a somewhat strong opinion that it's not an acceptable outcome for us to have a concise output format that's significantly worse than the concise output format of existing, established type checkers :-)

I also think we should weigh the experience of our users over the experience of us as developers. Both are important, of course, and obviously not every possible complication to our diagnostic model would be worth it just because it improves the user experience a little. But for me, I think this one clearly meets the bar.

---

_@MichaReiser approved on 2025-11-17 13:55_

---

_Comment by @MichaReiser on 2025-11-17 13:56_

As long as using this override remains the exception, I think it's fine.

---

_Comment by @sharkdp on 2025-11-17 15:54_

> I realize this is framed as also improving our verbose diagnostics, but I would consider relegating concise diagnostics to "best effort" only and improve the verbose diagnostic while mostly ignoring the quality of the concise output. But this is predicated on the assumption that the quality of the concise output isn't That Important, and perhaps my instincts are wrong there.

I wouldn't worry too much about concise diagnostic messages if they were mostly for our own use (mdtests, ecosystem results) and only rarely user-facing. But concise diagnostic messages are used very prominently in IDEs, and not all of them can show annotations. Here's neovim with ty `main` ("what is the inferred type?"):

<img width="861" height="50" alt="image" src="https://github.com/user-attachments/assets/3d28c847-77c6-4317-9ac9-e0b59c137d8c" />

---

_Comment by @MichaReiser on 2025-11-17 16:00_

> I wouldn't worry too much about concise diagnostic messages if they were mostly for our own use (mdtests, ecosystem results) and only rarely user-facing. But concise diagnostic messages are used very prominently in IDEs, and not all of them can show annotations. Here's neovim with ty main ("what is the inferred type?"):

Hmm, I think this raises a good point. The LSP now uses a combination of concise + related diagnostics that isn't tested on the CLI at all. 

I think we may want to change `to_lsp_diagnostic` to use the normal primary message if the client supports related information ([see](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnosticClientCapabilities))

---

_Merged by @sharkdp on 2025-11-18 08:35_

---

_Closed by @sharkdp on 2025-11-18 08:35_

---

_Branch deleted on 2025-11-18 08:35_

---

_Comment by @MichaReiser on 2025-11-18 08:44_

> use

Is this something you plan as a follow-up? If not, we should at least ensure that we create an issue

---

_Comment by @sharkdp on 2025-11-18 09:05_

Will open an issue later => https://github.com/astral-sh/ty/issues/1582

---
