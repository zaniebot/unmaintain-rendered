```yaml
number: 19662
title: "[ty] Improve debuggability of protocol types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/proto-interface
created_at: 2025-07-31T13:03:12Z
updated_at: 2025-08-01T14:16:14Z
url: https://github.com/astral-sh/ruff/pull/19662
synced_at: 2026-01-12T15:56:44Z
```

# [ty] Improve debuggability of protocol types

---

_@AlexWaygood_

## Summary

As our protocol implementation continues to progress, it's increasingly important for debuggability to know what types are being captured for each member in a protocol's interface and what kind of a member each member is treated as. One way of obtaining this information is to use a `dbg!()` call, but this means that you have to recompile ty (which takes a while), and also prints out a lot of detail that you don't need, making it hard to see the information that you do need.

This PR adds a `ty_extensions.reveal_protocol_interface` function to solve this issue. If you pass a protocol to this function, ty emits a diagnostic that prints a formatted representation of the underlying interface. 

## Test Plan

mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-07-31 13:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-31 13:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-31 13:03_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-31 13:03_

---

_Label `ty` added by @AlexWaygood on 2025-07-31 13:03_

---

_Label `internal` added by @AlexWaygood on 2025-07-31 13:03_

---

_Comment by @github-actions[bot] on 2025-07-31 13:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-01 14:10:34.764459119 +0000
+++ new-output.txt	2025-08-01 14:10:34.827459530 +0000
@@ -368,8 +368,8 @@
 generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr` and `AnyStr`
 generics_basic.py:139:5: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_basic.py:140:5: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_basic.py:157:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap1[str, int]`
-generics_basic.py:158:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap2[int, str].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap2[int, str]`
+generics_basic.py:157:5: error[call-non-callable] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` is not callable on object of type `MyMap1[str, int]`
+generics_basic.py:158:5: error[call-non-callable] Method `__getitem__` of type `bound method MyMap2[int, str].__getitem__(key: str, /) -> int` is not callable on object of type `MyMap2[int, str]`
 generics_basic.py:162:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Generic`
 generics_basic.py:163:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Protocol`
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
@@ -704,7 +704,6 @@
 namedtuples_usage.py:30:1: error[type-assertion-failure] Argument does not have asserted type `str`
 namedtuples_usage.py:31:1: error[type-assertion-failure] Argument does not have asserted type `int`
 namedtuples_usage.py:32:1: error[type-assertion-failure] Argument does not have asserted type `int`
-namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to object of type `Point` with no `__setitem__` method
 namedtuples_usage.py:49:1: error[type-assertion-failure] Argument does not have asserted type `int`
 namedtuples_usage.py:50:1: error[type-assertion-failure] Argument does not have asserted type `str`
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str]`
@@ -725,7 +724,7 @@
 narrowing_typeis.py:96:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
-overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int) -> int, (__s: slice[Any, Any, Any]) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
+overloads_basic.py:39:1: error[call-non-callable] Method `__getitem__` of type `Overload[(__i: int) -> int, (__s: slice[Any, Any, Any]) -> bytes]` is not callable on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
 overloads_definitions.py:33:5: error[invalid-overload] Overloaded non-stub function `func2` must have an implementation
 overloads_definitions.py:63:9: error[invalid-overload] Overloaded non-stub function `not_abstract` must have an implementation
@@ -889,4 +888,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 890 diagnostics
+Found 889 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-31 13:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:2286 on 2025-08-01 06:37_

```suggestion
    diagnostic.info("See https://typing.python.org/en/latest/spec/protocol.html");
```

---

_@sharkdp approved on 2025-08-01 06:40_

Cool. I can see how this can be very helpful. I like that you are constructing the "debug" representation manually instead of relying on `{:?}`.

Still, I'm a little worried that we should maybe be splitting `ty_extensions` into "public" and "definitely internal" parts (see my comment [here](https://github.com/astral-sh/ty/issues/902#issuecomment-3131244850)), because this definitely belongs in the latter category. But this is also true for other functions in `ty_extensions`, so not only related to this PR.

---

_Comment by @sharkdp on 2025-08-01 06:42_

Thinking a bit more about this... if we do want to eventually add something like `ty_extensions.reveal_type_debug`, couldn't this be part of the functionality? Whenever you call `reveal_type_debug` on a protocol, it would not just clearly state that it's a `Type::ProtocolInstance`, but also give information about the inner structure?

---

_Comment by @MichaReiser on 2025-08-01 07:15_

> Still, I'm a little worried that we should maybe be splitting ty_extensions into "public" and "definitely internal" parts (see my comment https://github.com/astral-sh/ty/issues/902#issuecomment-3131244850), because this definitely belongs in the latter category. But this is also true for other functions in ty_extensions, so not only related to this PR.

I think this is a fair concern, although I'm not sure if we should indeed communicate that anything about `ty_extension` is public in the first place or falls under our versioning policy. 

Either way. I guess the pythonic solution to this problem could be to mark the "more internal" functions as private: `ty_extension.__reveal_debug_tupe`?



---

_Comment by @sharkdp on 2025-08-01 07:22_

> Either way. I guess the pythonic solution to this problem could be to mark the "more internal" functions as private: `ty_extension.__reveal_debug_tupe`?

I think I'd prefer `from ty_extensions.internal import reveal_type_debug`, but no strong opinion.

---

_Comment by @AlexWaygood on 2025-08-01 14:11_

Yes, it would be nice to expose some parts of `ty_extensions` to the public at some point, but we'd need to think through quite a few things:
- How would we publish it to PyPI? Or would we skip that, and document that you can only import these in an `if TYPE_CHECKING` block?
- We'd need to switch from PEP-695 syntax to legacy generics if we want it to be valid syntax on Python <3.12
- There are a number of internal functions in there that we might not want to commit to maintaining as public API

So I agree that we have a number of internal functions in `ty_extensions` right now, that we shouldn't expose as public API. But to me this seems like a pre-existing problem; for now, I'd prefer to think of the whole module as private until we actively think through these issues and decide to commit to making some of them public API.

---

_Comment by @AlexWaygood on 2025-08-01 14:16_

> Thinking a bit more about this... if we do want to eventually add something like `ty_extensions.reveal_type_debug`, couldn't this be part of the functionality? Whenever you call `reveal_type_debug` on a protocol, it would not just clearly state that it's a `Type::ProtocolInstance`, but also give information about the inner structure?

It _could_ be. But I think I'd want more information from `reveal_type_debug`? There's a lot of information about the types of members that the diagnostic being added here is deliberately leaving out (for brevity), but I think for `reveal_type_debug` I'd want something similar to a `dbg!()` invocation. Just give me all the information I could possibly need, maybe something in there will tell me what's going on!!

---

_Merged by @AlexWaygood on 2025-08-01 14:16_

---

_Closed by @AlexWaygood on 2025-08-01 14:16_

---

_Branch deleted on 2025-08-01 14:16_

---
