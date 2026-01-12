```yaml
number: 20248
title: "[ty] Implement the legacy PEP-484 convention for indicating positional-only parameters"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex-carl/pep484-positionals
created_at: 2025-09-04T18:50:58Z
updated_at: 2025-09-05T16:56:07Z
url: https://github.com/astral-sh/ruff/pull/20248
synced_at: 2026-01-12T15:56:57Z
```

# [ty] Implement the legacy PEP-484 convention for indicating positional-only parameters

---

_@AlexWaygood_

## Summary

PEP 570, introduced in Python 3.8, added dedicated Python syntax for denoting positional-only
parameters (the `/` in a function signature). However, functions implemented in C were able to have positional-only parameters prior to Python 3.8 (there was just no syntax for expressing this at the Python level).

Stub files describing functions implemented in C nonetheless needed a way of expressing that certain parameters were positional-only. In the absence of dedicated Python syntax, PEP 484 described a convention that type checkers were expected to understand:

> Some functions are designed to take their arguments only positionally, and expect their callers never to use the argument’s name to provide that argument by keyword. All arguments with names beginning with `__` are assumed to be positional-only, except if their names also end with `__`.

While this convention is now redundant (following the implementation of PEP 570), many projects still continue to use the old convention. This PR therefore adds support for the older convention.

## Test Plan

Mdtests added and updated

---

Co-authored-by: Carl Meyer <carl@astral.sh>


---

_Label `ty` added by @AlexWaygood on 2025-09-04 18:51_

---

_Comment by @github-actions[bot] on 2025-09-04 18:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-05 11:42:40.616229139 +0000
+++ new-output.txt	2025-09-05 11:42:43.238286300 +0000
@@ -606,6 +606,10 @@
 generics_variance_inference.py:170:1: error[invalid-assignment] Object of type `ShouldBeInvariant6[int]` is not assignable to `ShouldBeInvariant6[int | float]`
 generics_variance_inference.py:181:1: error[invalid-assignment] Object of type `ShouldBeCovariant6[int | float]` is not assignable to `ShouldBeCovariant6[int]`
 generics_variance_inference.py:194:1: error[invalid-assignment] Object of type `ShouldBeContravariant2[int]` is not assignable to `ShouldBeContravariant2[int | float]`
+historical_positional.py:18:1: error[missing-argument] No argument provided for required parameter `__x` of function `f1`
+historical_positional.py:18:4: error[unknown-argument] Argument `__x` does not match any known parameter of function `f1`
+historical_positional.py:43:1: error[missing-argument] No argument provided for required parameter `__x` of bound method `m1`
+historical_positional.py:43:6: error[unknown-argument] Argument `__x` does not match any known parameter of bound method `m1`
 literals_interactions.py:15:5: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
 literals_interactions.py:16:5: error[index-out-of-bounds] Index -5 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
 literals_interactions.py:17:5: error[index-out-of-bounds] Index 4 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
@@ -692,7 +696,7 @@
 narrowing_typeis.py:96:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
-overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int) -> int, (__s: slice[Any, Any, Any]) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
+overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
 overloads_definitions.py:33:5: error[invalid-overload] Overloaded non-stub function `func2` must have an implementation
 overloads_definitions.py:63:9: error[invalid-overload] Overloaded non-stub function `not_abstract` must have an implementation
@@ -864,5 +868,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 865 diagnostics
+Found 869 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-04 18:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- tests/annotations/declarations.py:860:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:863:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:866:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 564 diagnostics
+ Found 561 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-09-04 22:31_

The conformance suite diff is all positive: we're emitting new diagnostics on lines where we're meant to emit diagnostics.

The [new hits on `hydra-zen`](https://github.com/mit-ll-responsible-ai/hydra-zen/blob/4a9997c6a9907d0c85a39a422d8f5760d7358b68/tests/annotations/declarations.py#L858-L866) are also expected -- they're actually trying to _assert_ on these lines that certain calls cause the type checker to emit diagnostics.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1246 on 2025-09-04 22:40_

I initially tried to see what would happen if we made `self` or `cls` _always_ implicitly positional-only, even if there were no other parameters in the function that used the PEP-484 convention. But there were a surprising number of ecosystem hits where people really are passing `self` or `cls` using keyword arguments.

The reason why this might be "interesting" for protocols is that [`Iterator.__iter__`](https://github.com/python/typeshed/blob/07557a4316d246b4315f600fd4c9734297d6bc92/stdlib/typing.pyi#L528) (for example) is not annotated as having a positional-only `self` parameter in typeshed. A naive implementation of protocol assignability/subtyping for method members might therefore lead us to conclude that this class isn't a subtype of `Iterable` (the protocol declares that it must be possible to pass `self` by keyword argument, and you can't do that with `Foo.__iter__`!), which is almost certainly going to be a counter-intuitive result for the user and go against their intent:

```py
from typing import Iterator

class Foo:
    def __iter__(self, /) -> Iterator[int]: ...
```

I suppose one alternative here might be to treat `self` as always implicitly positional-only for dunder methods? That doesn't feel like it would lead to particularly consistent or principled behaviour though.

I also think it's not essential to find a solution to this now, though. It's sort of a rare edge case, and anyway I'm pretty sure I can work around it fairly easily in our `Protocol` implementation

---

_@AlexWaygood reviewed on 2025-09-04 22:40_

---

_Marked ready for review by @AlexWaygood on 2025-09-04 22:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-04 22:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-04 22:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-04 22:40_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/definition.rs`:1137 on 2025-09-04 23:58_

We could fill this out and add similar `From` implementations for the `AstNodeRef` wrapper of every AST node kind that we currently implement `From` for a direct reference to. I intended in our pairing session today that we'd come back and do this; it might save someone some confusion in future if they try to use a different kind of `AstNodeRef` as an `Into<DefinitionNodeKey>`. Seems sort of odd to arbitrarily support it only for class definitions, even if that's the only place we currently use it.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:796 on 2025-09-05 00:18_

Nice! Glad to see we don't have to duplicate the source of truth for special-cased method names.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1246 on 2025-09-05 00:23_

It makes sense to me to imply this only for `Protocol` methods (as suggested in your `TODO` comment above.)

---

_@carljm approved on 2025-09-05 00:23_

Nice!

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-05 08:47_

---

_@AlexWaygood reviewed on 2025-09-05 11:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/definition.rs`:1137 on 2025-09-05 11:19_

I found a way to generalise this impl (🥳) using higher-ranked trait bounds (😱) in https://github.com/astral-sh/ruff/pull/20248/commits/487334cb1498aa5a48065d00543a16b8b6d9d10c

---

_@AlexWaygood reviewed on 2025-09-05 11:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1246 on 2025-09-05 11:20_

done in https://github.com/astral-sh/ruff/pull/20248/commits/3e1153a1a174fd4fdd9b79f865153be71394b994

---

_Comment by @github-actions[bot] on 2025-09-05 11:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @AlexWaygood on 2025-09-05 11:56_

Requesting a re-review since this has changed a fair bit since you last took a look!

---

_Review requested from @carljm by @AlexWaygood on 2025-09-05 11:56_

---

_@carljm approved on 2025-09-05 16:33_

This is great!

---

_Merged by @AlexWaygood on 2025-09-05 16:56_

---

_Closed by @AlexWaygood on 2025-09-05 16:56_

---

_Branch deleted on 2025-09-05 16:56_

---
