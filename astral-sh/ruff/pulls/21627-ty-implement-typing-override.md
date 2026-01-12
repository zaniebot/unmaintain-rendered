```yaml
number: 21627
title: "[ty] Implement `typing.override`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/explicit-override
created_at: 2025-11-25T12:57:44Z
updated_at: 2025-11-25T18:42:42Z
url: https://github.com/astral-sh/ruff/pull/21627
synced_at: 2026-01-12T15:57:29Z
```

# [ty] Implement `typing.override`

---

_@AlexWaygood_

## Summary

Part of https://github.com/astral-sh/ty/issues/155. This implements the basic check (`@override`-decorated methods should override things!), but not the strict check specified in https://typing.python.org/en/latest/spec/class-compat.html#strict-enforcement-per-project, which should be a separate error code.

## Test Plan

mdtests and snapshots


---

_Label `ty` added by @AlexWaygood on 2025-11-25 12:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-25 12:57_

---

_Label `ty` added by @AlexWaygood on 2025-11-25 12:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-25 12:57_

---

_Comment by @astral-sh-bot[bot] on 2025-11-25 12:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-25 18:04:28.872627110 +0000
+++ new-output.txt	2025-11-25 18:04:32.604642480 +0000
@@ -194,6 +194,10 @@
 classes_classvar.py:77:8: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
 classes_classvar.py:111:1: error[invalid-attribute-access] Cannot assign to ClassVar `stats` from an instance of type `Starship`
 classes_classvar.py:140:13: error[invalid-assignment] Object of type `ProtoAImpl` is not assignable to `ProtoA`
+classes_override.py:53:9: error[invalid-explicit-override] Method `method3` is decorated with `@override` but does not override anything
+classes_override.py:65:9: error[invalid-explicit-override] Method `method4` is decorated with `@override` but does not override anything
+classes_override.py:79:9: error[invalid-explicit-override] Method `static_method1` is decorated with `@override` but does not override anything
+classes_override.py:84:9: error[invalid-explicit-override] Method `class_method1` is decorated with `@override` but does not override anything
 constructors_call_init.py:21:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `float`
 constructors_call_init.py:25:1: error[type-assertion-failure] Type `Class1[int | float]` does not match asserted type `Class1[float]`
 constructors_call_init.py:56:1: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Class4[int]`, found `Class4[str]`
@@ -776,6 +780,7 @@
 overloads_definitions.py:159:46: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:167:44: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:183:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
+overloads_definitions.py:198:9: error[invalid-explicit-override] Method `bad_override` is decorated with `@override` but does not override anything
 overloads_definitions.py:198:45: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:215:46: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | str`
 overloads_definitions.py:229:9: error[invalid-overload] `@override` decorator should be applied only to the overload implementation
@@ -786,6 +791,7 @@
 overloads_definitions_stub.pyi:44:9: error[invalid-overload] Overloaded function `func6` does not use the `@classmethod` decorator consistently
 overloads_definitions_stub.pyi:76:9: error[invalid-overload] `@final` decorator should be applied only to the first overload
 overloads_definitions_stub.pyi:86:9: error[invalid-overload] `@final` decorator should be applied only to the first overload
+overloads_definitions_stub.pyi:122:9: error[invalid-explicit-override] Method `bad_override` is decorated with `@override` but does not override anything
 overloads_definitions_stub.pyi:147:9: error[invalid-overload] `@override` decorator should be applied only to the first overload
 overloads_evaluation.py:38:1: error[no-matching-overload] No overload of function `example1_1` matches arguments
 overloads_evaluation.py:46:15: error[invalid-argument-type] Argument to function `example1_1` is incorrect: Expected `str`, found `Literal[1]`
@@ -1005,5 +1011,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1007 diagnostics
+Found 1013 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-25 13:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
graphql-core (https://github.com/graphql-python/graphql-core)
- tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str] & dict[str, Any]`
+ tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & dict[Unknown | str, Unknown | str]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]] & list[tuple[str, str, str] | tuple[str, int, Unknown]]`
+ ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[tuple[str, str, str] | tuple[str, int, Unknown]] & list[Unknown | tuple[str, int, Unknown]]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-25 13:06_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://alex-explicit-override.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-explicit-override.ecosystem-663.pages.dev/timing))




---

_Closed by @AlexWaygood on 2025-11-25 13:07_

---

_Reopened by @AlexWaygood on 2025-11-25 13:08_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-25 13:11_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-25 13:11_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-25 14:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-25 14:06_

---

_Marked ready for review by @AlexWaygood on 2025-11-25 14:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-25 14:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-25 14:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-25 14:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-25 14:54_

---

_Comment by @AlexWaygood on 2025-11-25 14:55_

The typing conformance suite results are all good -- all the places where we're newly emitting diagnostics are marked with `# E`.

The ecosystem results are also excellent -- this is one of the few new diagnostics where you'd really expect _no_ new ecosystem hits, and indeed that's what we have!

---

_@MichaReiser reviewed on 2025-11-25 15:02_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 15:02_

 I think I'd prefer `invalid-override` or `invalid-explicit-override`. `explicit-override` sounds like explicit overrides are a bad thing.

And `incorrect-override` for an override that doesn't match the signature of the parent class

---

_@AlexWaygood reviewed on 2025-11-25 15:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:170 on 2025-11-25 15:02_

methods on `TypedDict` subclasses are invalid (as noted in comments in `liskov.md`)... but they exist in the wild anyway, and some of them are decorated with `@override`: https://github.com/Toufool/AutoSplit/blob/d9c5e3b48eeda52a13c08e75b5ffd61873ce5b8f/src/user_profile.py#L50-L53

---

_@AlexWaygood reviewed on 2025-11-25 15:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 15:03_

yeah, I wasn't sure what to call this. I don't want to call it `invalid-override` because that implies a Liskov violation to me (e.g. the name for the "bad method override" diagnostic is `invalid-method-override`).

`invalid-explicit-override` is okay, I can switch to that. (Edit: I pushed this rename.)

---

_@AlexWaygood reviewed on 2025-11-25 15:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/override.md`:220 on 2025-11-25 15:08_

It's a bit annoying to implement this but it means that we pass several typing conformance-suite tests that we'd otherwise fail, so I think it's worth it

---

_@MichaReiser reviewed on 2025-11-25 15:15_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 15:15_

Hmm yeah, this is tricky. `invalid-explicit-override` is a bit long.

Should `invalid-method-override` be `incorrect-method-override` to make the distinction clearer? What name do you have in mind for the strict enforcement?

* Pyrefly: bad-override

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 15:30_

> `invalid-explicit-override` is a bit long.

not as long as some of the ones we already have, such as `subclass-of-final-class` 😄

> Should `invalid-method-override` be `incorrect-method-override` to make the distinction clearer?

I'm not sure that would make the distinction much clearer. Applying `@override` to a method that doesn't override anything is just as incorrect as overriding a method in a way that violates the Liskov Substitution Principle.

> What name do you have in mind for the strict enforcement?

The one I'm using in my head right now is `invalid-explicit-override-strict` or similar. But it's not great.

Another option here would be for the strict enforcement would actually just be the same error code, but we'd have a `tool.ty.explicit-override-enforcement = true` configuration setting (etc.) that allows you to opt into the stricter checks. That would obviate the need for a separate error code, which is nice. But the strict enforcement is really detecting a conceptually pretty different (and far more pedantic) problem -- so a separate error code does feel appropriate in some ways.

Do you have any suggestions?

---

_@AlexWaygood reviewed on 2025-11-25 15:30_

---

_@AlexWaygood reviewed on 2025-11-25 15:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 15:32_

Other possible names for the Liskov violation code might be:
- `unsound-method-override`
- `liskov-violating-override`
- `incompatible-method-override`

But I think even if we went with any of those for the Liskov violation error, I would still want the term "explicit" somewhere in the error code for invalid uses of `typing.override`, so that we're clear that _this_ error code isn't about overrides in general, it's about overrides that are demarcated with `typing.override`.

---

_@AlexWaygood reviewed on 2025-11-25 15:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 15:34_

I do also think that the most important thing is to get this check in soon. It's very easy to change the name of an error code later, especially while we still declare ty to be alpha or beta software

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/override.md`:180 on 2025-11-25 17:29_

```suggestion
implementation function. However, we nonetheless respect the decorator in this situation, even
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/override.md`:287 on 2025-11-25 17:42_

I don't think an `invalid-explicit-override` would be _wrong_ here, exactly -- `NamedTuple` isn't really a base class, so you aren't overriding anything here. But I think it's fine if we don't emit it.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1580 on 2025-11-25 17:46_

IMO `invalid-explicit-override` is fine for this code. I think for the strict check the right name would be `invalid-implicit-override`, since the nature of the strict check is "you failed to use `@override` here, but you are required to" -- that is, you are making an implicit override, but overrides are required to be explicit.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:132 on 2025-11-25 17:57_


I'm not sure why this was changed? All tests pass if I change this back to `break`. The previous rationale (and comment) still seem correct to me. I think continuing to iterate the MRO after we've established that `Type::member` returns undefined (which internally already checked the rest of the MRO) is wasteful?

```suggestion
            // If not defined on any superclass, no point in continuing to walk up the MRO
            break;
```

---

_@carljm approved on 2025-11-25 18:00_

Looks good to me!

---

_Merged by @carljm on 2025-11-25 18:42_

---

_Closed by @carljm on 2025-11-25 18:42_

---

_Branch deleted on 2025-11-25 18:42_

---
