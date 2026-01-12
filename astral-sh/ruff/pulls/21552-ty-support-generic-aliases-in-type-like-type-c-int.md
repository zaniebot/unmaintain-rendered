```yaml
number: 21552
title: "[ty] support generic aliases in `type[...]`, like `type[C[int]]`"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/type_slice_slice
created_at: 2025-11-21T00:42:24Z
updated_at: 2025-11-24T21:56:44Z
url: https://github.com/astral-sh/ruff/pull/21552
synced_at: 2026-01-12T15:57:27Z
```

# [ty] support generic aliases in `type[...]`, like `type[C[int]]`

---

_@oconnor663_

Closes https://github.com/astral-sh/ty/issues/1101.

As I push this PR, there is [one failing test line](https://github.com/astral-sh/ruff/blob/06941c1987bd4fe2b54d99b5514784472cabc0b3/crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md?plain=1#L244) on my box, which I need help interpreting. Here's an extracted version of it:

```py
from ty_extensions import static_assert, is_assignable_to, TypeOf
from typing import Generic, TypeVar

T_co = TypeVar("T_co", covariant=True)

class Foo(Generic[T_co]): ...
class Bar(Foo[T_co], Generic[T_co]): ...

static_assert(is_assignable_to(TypeOf[Bar], type[Foo[int]]))  # error: false
```

That `static_assert` fails. The "never" result seems to originate on [this line](https://github.com/astral-sh/ruff/blob/06941c1987bd4fe2b54d99b5514784472cabc0b3/crates/ty_python_semantic/src/types/class.rs#L591), which I read as "no non-generic class literal is ever assignable/etc to a specialization" (not sure how to word that correctly). That...seems not true...?...so I'm wondering I've somehow skipped the step where the non-generic class literal, which does in fact have generic parameters, gets them implicitly specialized to `object`? I'm betting this will be super obvious to someone :)

---

_Review requested from @carljm by @oconnor663 on 2025-11-21 00:42_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-11-21 00:42_

---

_Review requested from @sharkdp by @oconnor663 on 2025-11-21 00:42_

---

_Review requested from @dcreager by @oconnor663 on 2025-11-21 00:42_

---

_Comment by @oconnor663 on 2025-11-21 00:43_

Also thanks @carljm for talking me through the implementation of this :)

---

_Label `ty` added by @oconnor663 on 2025-11-21 00:43_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 00:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-24 20:07:31.699340208 +0000
+++ new-output.txt	2025-11-24 20:07:35.435382789 +0000
@@ -220,6 +220,7 @@
 constructors_call_new.py:117:1: error[type-assertion-failure] Type `Class8[list[str]]` does not match asserted type `Class8[str]`
 constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__new__`
 constructors_call_new.py:140:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class11[int]`
+constructors_call_new.py:145:1: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `type[Class11[int]]`, found `<class 'Class11[str]'>`
 constructors_call_type.py:40:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of function `__new__`
 constructors_call_type.py:50:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of bound method `__init__`
 constructors_call_type.py:59:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
@@ -415,25 +416,26 @@
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:172:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:199:5: error[type-assertion-failure] Type `Iterator[Any]` does not match asserted type `Unknown`
-generics_defaults.py:30:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults'>`
-generics_defaults.py:31:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
-generics_defaults.py:32:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
-generics_defaults.py:38:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'OneDefault[int | float, bool]'>`
-generics_defaults.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults'>`
-generics_defaults.py:46:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:30:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults'>`
+generics_defaults.py:31:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
+generics_defaults.py:32:1: error[type-assertion-failure] Type `type[NoNonDefaults[str, int]]` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
+generics_defaults.py:38:1: error[type-assertion-failure] Type `type[OneDefault[int | float, bool]]` does not match asserted type `<class 'OneDefault[int | float, bool]'>`
+generics_defaults.py:45:1: error[type-assertion-failure] Type `type[AllTheDefaults[Any, Any, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults'>`
+generics_defaults.py:46:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:50:1: error[missing-argument] No argument provided for required parameter `T2` of class `AllTheDefaults`
-generics_defaults.py:52:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:55:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:59:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:63:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:79:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Class_ParamSpec'>`
+generics_defaults.py:52:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:55:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:59:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:63:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
+generics_defaults.py:79:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `<class 'Class_ParamSpec'>`
+generics_defaults.py:79:35: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:80:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:80:32: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:81:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
 generics_defaults.py:81:46: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Class_TypeVarTuple'>`
+generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `<class 'Class_TypeVarTuple'>`
 generics_defaults.py:95:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `Class_TypeVarTuple`
 generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
 generics_defaults.py:131:1: error[type-assertion-failure] Type `Any` does not match asserted type `int`
@@ -442,15 +444,15 @@
 generics_defaults.py:155:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
 generics_defaults.py:156:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
 generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth(self, /) -> Self@meth`
-generics_defaults_referential.py:23:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'slice'>`
+generics_defaults_referential.py:23:1: error[type-assertion-failure] Type `type[slice[int, int, int | None]]` does not match asserted type `<class 'slice'>`
 generics_defaults_referential.py:36:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
-generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
-generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar[int, list[int]]'>`
+generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `type[Bar[Any, list[Any]]]` does not match asserted type `<class 'Bar'>`
+generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `type[Bar[int, list[int]]]` does not match asserted type `<class 'Bar[int, list[int]]'>`
 generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
 generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
-generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
+generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `type[Bar[str]]` does not match asserted type `<class 'Bar'>`
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
 generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -1003,5 +1005,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1005 diagnostics
+Found 1007 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @carljm on 2025-11-21 01:45_

The implicit specialization should use the `default_specialization` method, and should not specialize to `object` but to `Unknown` (or the typevar default, if any, but there is none in this example).

To be honest I'm not sure if we want `TypeOf[Bar]` to implicitly default-specialize `Bar`, though. `TypeOf` is a test introspection helper designed to say "give me the type for this name", and I think it might break some other uses of it if it always implicitly default-specialized instead of giving the "raw" type of that name.

So I suspect the better option here might be to change that test assertion, with a comment saying something like "`TypeOf` does not implicitly default-specialize. The unspecialized class literal object `Bar` does not inhabit the type `type[Foo[int]]`."

And then add another test asserting explicitly that `type[Bar[Unknown]]` is assignable to `type[Foo[int]]`, if we don't already have that version.

I think the line you pointed to in `ClassType::has_relation_to_impl` is correct. What it is saying is that I am only a subclass of `other` if I inherit from a base class with matching specialization with `other` -- if `other` is generic and my base is not (or vice versa), that cannot make me a subclass of `other`. Note that we are in a `when_any(...)` iteration of `self.iter_mro(db)` there.

---

_@oconnor663 reviewed on 2025-11-21 17:47_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:246 on 2025-11-21 17:47_

Is this new `Unknown` case redundant with the `Any` case right above it, or are is their behavior different enough that it makes sense to test both?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:246 on 2025-11-21 18:44_

It's redundant, we very rarely handle `Unknown` differently from `Any`. But there's also no harm in including it, don't really care either way.

---

_@carljm approved on 2025-11-21 18:44_

Ship it!

---

_Comment by @AlexWaygood on 2025-11-21 18:54_

> Ship it!

Looks like we're panicking on a mypy_primer project in CI?

---

_Comment by @carljm on 2025-11-21 19:07_

Nice catch :) Ok then, don't ship it quite yet...

---

_Comment by @oconnor663 on 2025-11-21 19:59_

What's one little panic between friends?

---

_Comment by @oconnor663 on 2025-11-21 21:30_

Progress, this seems to be a minimal repro:

```py
class Foo:
    pass

_: type[Foo[int]]
```

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:6946:14 when checking `/tmp/test.py`: `assertion `left == right` failed
  left: Some(ClassLiteral(ClassLiteral { name: Name("Foo"), body_scope: ScopeId { [salsa id]: Id(1001), file: File(System("/tmp/test.py")), file_scope_id: FileScopeId(1) }, known: None, deprecated: None, type_check_only: false, dataclass_params: None, dataclass_transformer_params: None }))
 right: None`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: ruff/0.14.5+81 (502618634 2025-11-21)
info: Args: ["/home/jacko/astral/ruff/target-mold/debug/ty", "check", "test.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(1401))
             at crates/ty_python_semantic/src/types/infer.rs:94
   1: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
   2: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535
```

I think this is the panic that we get when something is inferred more than once. Did it used to have a better error message than this?

---

_@carljm reviewed on 2025-11-21 21:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:728 on 2025-11-21 21:32_

This looks like the likely cause of the panic, `slice` includes `value` which we already inferred above.

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:728 on 2025-11-21 21:33_

Yep already there.

---

_@oconnor663 reviewed on 2025-11-21 21:33_

---

_@oconnor663 reviewed on 2025-11-21 21:37_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:728 on 2025-11-21 21:37_

Interesting that the line in the wild that triggers this is https://github.com/python-jsonschema/referencing/blob/d97e65e86c95d51f01e18d89461da6d4a0c4e72a/referencing/_core.py#L200

```py
        default_specification: (
            type[Specification[D]] | Specification[D]
        ) = Specification,
```

But in this context, `Specification` _is_ generic...

---

_@carljm reviewed on 2025-11-21 21:47_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:728 on 2025-11-21 21:47_

Oh, that's strange and deserving of further investigation?

But doesn't need to block landing this IMO.

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
- src/pip/_vendor/truststore/_macos.py:294:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/pip/_vendor/truststore/_macos.py:314:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/pip/_vendor/truststore/_macos.py:337:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 603 diagnostics
+ Found 600 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ tests/test_formparser.py:437:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[MultiDict[str, Any]] | None`, found `<class 'dict'>`
- Found 398 diagnostics
+ Found 399 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/capture.py:940:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/_pytest/capture.py:941:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 459 diagnostics
+ Found 461 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/config.py:5862:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 97 diagnostics
+ Found 96 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/collections/__init__.pyi:43:11: error[too-many-positional-arguments] Too many positional arguments to class `tuple`: expected 1, got 2
- Found 1743 diagnostics
+ Found 1744 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg/psycopg/connection.py:78:9: error[invalid-assignment] Object of type `<class 'Cursor'>` is not assignable to attribute `cursor_factory` of type `type[Cursor[Row@Connection]]`
+ psycopg/psycopg/connection.py:79:9: error[invalid-assignment] Object of type `<class 'ServerCursor'>` is not assignable to attribute `server_cursor_factory` of type `type[ServerCursor[Row@Connection]]`
+ psycopg/psycopg/connection_async.py:82:9: error[invalid-assignment] Object of type `<class 'AsyncCursor'>` is not assignable to attribute `cursor_factory` of type `type[AsyncCursor[Row@AsyncConnection]]`
+ psycopg/psycopg/connection_async.py:83:9: error[invalid-assignment] Object of type `<class 'AsyncServerCursor'>` is not assignable to attribute `server_cursor_factory` of type `type[AsyncServerCursor[Row@AsyncConnection]]`
+ psycopg/psycopg/types/composite.py:539:12: error[invalid-return-type] Return type does not match returned value: expected `type[_CompositeLoader[T@_make_loader]]`, found `type`
+ psycopg/psycopg/types/composite.py:556:12: error[invalid-return-type] Return type does not match returned value: expected `type[_CompositeBinaryLoader[T@_make_binary_loader]]`, found `type`
+ psycopg/psycopg/types/composite.py:575:12: error[invalid-return-type] Return type does not match returned value: expected `type[_SequenceDumper[T@_make_dumper]]`, found `type`
+ psycopg/psycopg/types/composite.py:594:12: error[invalid-return-type] Return type does not match returned value: expected `type[_SequenceBinaryDumper[T@_make_binary_dumper]]`, found `type`
+ psycopg/psycopg/types/enum.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumLoader[E@_make_loader]]`, found `type`
+ psycopg/psycopg/types/enum.py:191:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumLoader[E@_make_binary_loader]]`, found `type`
+ psycopg/psycopg/types/enum.py:199:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumDumper[E@_make_dumper]]`, found `type`
+ psycopg/psycopg/types/enum.py:207:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumDumper[E@_make_binary_dumper]]`, found `type`
+ psycopg/psycopg/types/multirange.py:407:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeLoader[Any]]`, found `type`
+ psycopg/psycopg/types/multirange.py:412:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeBinaryLoader[Any]]`, found `type`
+ psycopg/psycopg/types/range.py:586:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeLoader[Any]]`, found `type`
+ psycopg/psycopg/types/range.py:591:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeBinaryLoader[Any]]`, found `type`
- Found 663 diagnostics
+ Found 679 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/_post_coinit/activeobj.py:45:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/_post_coinit/activeobj.py:46:15: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/_post_coinit/misc.py:106:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/_post_coinit/misc.py:142:15: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/client/__init__.py:29:80: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/errorinfo.py:90:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/errorinfo.py:97:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ comtypes/safearray.py:393:13: error[invalid-super-argument] `Self@__setitem__` is not an instance or subclass of `type[_Pointer[Unknown]]` in `super(type[_Pointer[Unknown]], Self@__setitem__)` call
+ comtypes/test/test_basic.py:31:30: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `AddRef`
+ comtypes/test/test_basic.py:33:30: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `Release`
+ comtypes/test/test_basic.py:40:26: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `AddRef`
+ comtypes/test/test_basic.py:41:26: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `Release`
+ comtypes/test/test_basic.py:43:17: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `QueryInterface`
+ comtypes/test/test_basic.py:45:26: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `AddRef`
+ comtypes/test/test_basic.py:46:26: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `Release`
+ comtypes/test/test_basic.py:89:9: error[unresolved-attribute] Object of type `type[_Pointer[Unknown]]` has no attribute `QueryInterface`
- comtypes/test/test_comobject.py:78:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/test/test_comobject.py:79:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/test/test_comobject.py:80:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ comtypes/test/test_outparam.py:62:12: error[unresolved-attribute] Object of type `_Pointer[Unknown]` has no attribute `DidAlloc`
- comtypes/typeinfo.py:291:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:439:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:468:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:659:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:674:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:683:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:690:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:697:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- comtypes/typeinfo.py:722:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 365 diagnostics
+ Found 356 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:527:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ bson/codec_options.py:400:45: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type`, found `object`
- bson/regex.py:83:16: error[invalid-return-type] Return type does not match returned value: expected `Regex[_T@Regex]`, found `Regex[str]`
+ bson/son.py:43:31: error[invalid-assignment] Object of type `<class 'Pattern[str]'>` is not assignable to `<class 'Pattern[Any]'>`
- pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
+ pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Unknown | None | bytes`
- pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: Unknown, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: Unknown, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
+ pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Unknown | None | bytes`
- pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: Unknown, conn: Connection) -> None) | ((credentials: Unknown, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> None]`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:668:8: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/annotations/behaviors.py:50:29: error[unresolved-attribute] Object of type `Path` has no attribute `x`
+ tests/annotations/behaviors.py:54:26: error[invalid-assignment] Object of type `Path` is not assignable to `int`
- tests/annotations/declarations.py:122:9: info[revealed-type] Revealed type: `@Todo`
+ tests/annotations/declarations.py:122:9: info[revealed-type] Revealed type: `type[@Todo]`
- tests/annotations/declarations.py:233:9: info[revealed-type] Revealed type: `@Todo`
+ tests/annotations/declarations.py:233:9: info[revealed-type] Revealed type: `type[@Todo]`
+ tests/annotations/declarations.py:501:16: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `type[@Todo]`
+ tests/annotations/declarations.py:503:16: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `type[@Todo]`
- tests/annotations/mypy_checks.py:18:5: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:18:5: error[type-assertion-failure] Type `A` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:19:5: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:19:5: error[type-assertion-failure] Type `A` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:22:5: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:22:5: error[type-assertion-failure] Type `str` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:23:5: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:23:5: error[type-assertion-failure] Type `str` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:42:5: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:42:5: error[type-assertion-failure] Type `A` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:43:5: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:43:5: error[type-assertion-failure] Type `A` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:46:5: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:46:5: error[type-assertion-failure] Type `str` does not match asserted type `Path`
- tests/annotations/mypy_checks.py:47:5: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
+ tests/annotations/mypy_checks.py:47:5: error[type-assertion-failure] Type `str` does not match asserted type `Path`
- Found 547 diagnostics
+ Found 550 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/protocol/__init__.py:131:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 706 diagnostics
+ Found 705 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index_datetime.py:708:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1949 diagnostics
+ Found 1948 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/stats/_multicomp.py:69:22: warning[possibly-missing-attribute] Attribute `low` may be missing on object of type `@Todo | None`
+ scipy/stats/_multicomp.py:69:22: warning[possibly-missing-attribute] Attribute `low` may be missing on object of type `Unknown | None`
- scipy/stats/_multicomp.py:70:22: warning[possibly-missing-attribute] Attribute `high` may be missing on object of type `@Todo | None`
+ scipy/stats/_multicomp.py:70:22: warning[possibly-missing-attribute] Attribute `high` may be missing on object of type `Unknown | None`

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/types.py:852:12: error[invalid-return-type] Return type does not match returned value: expected `type[set[HashableItemType@conset]]`, found `<typing.Annotated special form>`
+ pydantic/types.py:868:12: error[invalid-return-type] Return type does not match returned value: expected `type[frozenset[HashableItemType@confrozenset]]`, found `<typing.Annotated special form>`
+ pydantic/types.py:903:12: error[invalid-return-type] Return type does not match returned value: expected `type[list[AnyItemType@conlist]]`, found `<typing.Annotated special form>`
- Found 6710 diagnostics
+ Found 6713 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @oconnor663 on 2025-11-21 22:07_

CI is green. Looking at some of the new diagnostics. For example [in `werkzeug`](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/sansio/request.py#L75):

```py
    parameter_storage_class: type[MultiDict[str, t.Any]] = ImmutableMultiDict
```

It seems like that's a true positive, and that the annotation should be `type[MultiDict]`? (Alternatively the assigned value could be `ImmutableMultiDict[str, t.Any`, but I think they want an actual class there? Not totally sure.)

---

_Comment by @carljm on 2025-11-21 22:37_

Hmmm, no, I think those ecosystem results might suggest we need to make an adjustment here... and I might have been wrong on the original question about the failing test line. I think we need to default-specialize in that assignment and consider the class object `ImmutableMultiDict` to have the type `type[ImmutableMultiDict[Unknown, Unknown]]`, which is assignable to `type[MultiDict[str, Any]]`.

What's not totally clear to me is at which level we should do that default specialization. Should we do it on the name load? At the assignability check?

---

_Comment by @carljm on 2025-11-21 22:38_

The conformance suite results are showing a similar problem. I think the use of `assert_type` in those tests is a bit questionable, but it's showing the same problem -- that we aren't default-specializing a generic class literal when the class object is mentioned.

---

_Comment by @oconnor663 on 2025-11-21 23:20_

At the risk of biting off more than I can chew, do we want to revisit [this](https://github.com/astral-sh/ruff/blob/ddc1417f22599ceaa998c1ee4053c657afb2d577/crates/ty_python_semantic/src/types/class.rs#L610-L614):

```rs
            // A non-generic class is never equivalent to a generic class.
            // Two non-generic classes are only equivalent if they are equal (handled above).
            (ClassType::NonGeneric(_), _) | (_, ClassType::NonGeneric(_)) => {
                ConstraintSet::from(false)
            }
```

I'll try it just for kicks...

---

_Comment by @carljm on 2025-11-22 00:38_

No, I don't think that's the right location for the fix. 

I think the problem is that we sometimes use `ClassType::NonGeneric` to represent an unspecialized generic class, and I think we should never(?) do that. `ClassType::NonGeneric` should always be an actual non-generic class type, and `ClassType::Generic` should always be a specialized generic class type. Because a true "class type" for a generic class must always be specialized, even if only default-specialized.

We have `Type::ClassLiteral` to represent a class literal object (non-generic or generic, but never specialized), so that when you say `MyGenericClass[X]` (an explicit specialization of `MyGenericClass`) or `MyGenericClass(X)` (a constructor call of `MyGenericClass` for which we will infer a specialization), we have some type for `MyGenericClass` in that expression that knows it isn't specialized already. So we can't just universally make all name-loads of `MyGenericClass` eagerly default-specialize themselves. But `Type::ClassLiteral` doesn't wrap `ClassType`, it directly wraps `ClassLiteral`, so I think that's still consistent with the idea that we should avoid ever constructing a `ClassType` that attempts to represent an unspecialized generic class.

I think maybe the narrow version of the fix here would be located at https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types.rs#L2381 -- that is at least one place where we are clearly constructing a `ClassType::NonGeneric` around an arbitrary `ClassLiteral`, with no regard for whether it is actually generic or not. Instead I think we should transform our `ClassLiteral` into a `ClassType` using a `ClassType` constructor (which AFAICS doesn't currently exist on `ClassType`, but I think should) which checks if the `ClassLiteral` is generic, and if so default-specializes it and constructs a `ClassType::Generic` around it, only otherwise constructing a `ClassType::NonGeneric`.

This fix is essentially saying that we consider `Type::ClassLiteral(Foo)` to be a subtype of `Type::SubclassOf(<default specialization of Foo>)` -- it's equivalent to the default specialization of `Foo` itself, excluding subclasses. I think this is a reasonable way to implement the expectation that any mention of `Foo` which _isn't_ an explicit specialization or a constructor call, should be considered the default specialization.

I suspect that fix may be enough to get the werkzeug example passing, and (depending what the rest of the ecosystem looks like) may be enough to make this landable.

The next follow-up (which could be interesting to try here, in case it just works smoothly, but if it reveals more hard problems may be better deferred) would be to actually add some assertions in `ClassType` which enforce the invariant that we never make a `ClassType::NonGeneric` around a generic `ClassLiteral`.

The next thing which we'd want to look at soon is fixing those conformance suite results. I think we may want to actually make an upstream pull request to the conformance suite to adjust those tests, because I think the way they are written is over-specified. They are asserting that the expression `Foo` (where `Foo` is a generic class) must have precisely the type `type[Foo[Unknown]]` (or whatever the default specialization of `Foo` might be). I think this is over-prescriptive, because it is valid for us to infer a narrower type here. What the conformance suite really should be asserting here is just assignability behavior, not precise types.

---

_Comment by @oconnor663 on 2025-11-24 18:04_

[83d2bc2](https://github.com/astral-sh/ruff/pull/21552/commits/83d2bc237cd167dc99d0b8e5235de4a21c82455f) has cleared most of the new `werkzeug` errors. I'm playing with relaxing the conformance suite now to see how that looks (not necessarily to change in this PR, but to see where this PR stands if we were to change it).

I think the remaining new `werkzeug` error is valid:

```
+ tests/test_formparser.py:437:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[MultiDict[str, Any]] | None`, found `<class 'dict'>`
```

I think the `cls=dict` argument [here](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/tests/test_formparser.py#L437) is indeed incorrect, because it's [supposed to be](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/formparser.py#L297) a `type[MultiDict[...]]`, but `MultiDict` [inherits from](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/datastructures/structures.py#L137) `TypeConversionDict` which [inherits from](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/datastructures/structures.py#L57) `dict`. We weren't surfacing that until this PR because we didn't parse `type[MultiDict[...]]`.

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-11-24 18:39_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 18:47_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 0 | 27 | 0 |
| `invalid-return-type` | 17 | 1 | 0 |
| `unresolved-attribute` | 10 | 0 | 0 |
| `invalid-assignment` | 6 | 0 | 2 |
| `type-assertion-failure` | 0 | 0 | 8 |
| `invalid-argument-type` | 4 | 0 | 2 |
| `possibly-missing-attribute` | 0 | 0 | 2 |
| `invalid-super-argument` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| **Total** | **39** | **28** | **14** |

**[Full report with detailed diff](https://jack-type-slice-slice.ecosystem-663.pages.dev/diff)** ([timing results](https://jack-type-slice-slice.ecosystem-663.pages.dev/timing))




---

_Comment by @oconnor663 on 2025-11-24 19:15_

@AlexWaygood has clarified that this `mongo-python-driver` ecosystem hit:
<img width="2166" height="214" alt="image" src="https://github.com/user-attachments/assets/79a2a0ac-82d4-4f8e-9de1-b53264e8574b" />
is an instance of this known issue: https://github.com/astral-sh/ty/issues/1513

---

_Comment by @oconnor663 on 2025-11-24 19:28_

@AlexWaygood again re: the `mypy` ecosystem hit:

> that diagnostic is being surfaced on mypy's vendored copy of typeshed. I think that's just us failing to recognise that mypy (like ty) is vendoring typeshed, and that therefore the tuple in mypy/typeshed/stdlib/builtins.pyi is builtins.tuple, and therefore gets to be afforded the same very special rules that the tuple class in ty_vendored/vendor/typeshed/stdlib/builtins.pyi is afforded

---

_Comment by @oconnor663 on 2025-11-24 20:22_

@carljm points out that `psycopg` errors like...
```
Object of type `<class 'Cursor'>` is not assignable to attribute `cursor_factory` of type `type[Cursor[Row@Connection]]`
```
...are legitimate, because `Row` is a typevar with a `default` there, so it doesn't default to `Unknown`. Separate question whether we actually like this `ty` behavior, but it's not new in this PR. (Edit: could be something that bidirectional type inference helps with in the future.)

---

_Comment by @carljm on 2025-11-24 21:05_

A bunch of the other new diagnostics in the ecosystem derive from https://github.com/astral-sh/ty/issues/740

---

_Comment by @carljm on 2025-11-24 21:05_

I filed https://github.com/astral-sh/ty/issues/1624 for follow-up on the psycopg2 issue mentioned above

---

_@carljm approved on 2025-11-24 21:14_

Looks good to me!

---

_Merged by @oconnor663 on 2025-11-24 21:56_

---

_Closed by @oconnor663 on 2025-11-24 21:56_

---

_Branch deleted on 2025-11-24 21:56_

---
