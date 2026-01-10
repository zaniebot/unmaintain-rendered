```yaml
number: 19950
title: "[ty] Fix protocol interface inference for stub protocols and subprotocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/subproto
created_at: 2025-08-17T17:10:41Z
updated_at: 2025-08-19T15:13:53Z
url: https://github.com/astral-sh/ruff/pull/19950
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix protocol interface inference for stub protocols and subprotocols

---

_Pull request opened by @AlexWaygood on 2025-08-17 17:10_

## Summary

This PR fixes two bugs in our inference of protocol interfaces.

Firstly, we currently correctly record whether a protocol member is marked as a `ClassVar`... but only if the protocol originates from a `.py` file! If the protocol originated from a `.pyi` file, we were accidentally throwing this information away -- this is due to the fact that we iterate over both declarations and bindings to infer a protocol interface, and in a stub file `x: ClassVar[int]` is considered both a declaration _and_ a binding, but the bindings iterator doesn't return any information about the qualifiers, and the information from the bindings iterator was overwriting any qualifiers information we'd obtained from the declarations iterator.

Secondly, we currently get subprotocols very wrong. For a situation like this, we infer `SubProto` as having a method member `method` that returns `int`, not `bool`, because we're incorrectly overwriting the information regarding the subprotocol with information about the parent protocol:

```py
from typing import Protocol

class BaseProto(Protocol):
    def method(self) -> int: ...

class SubProto(BaseProto, Protocol):
    def method(self) -> bool: ...
```

## Test Plan

Mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-08-17 17:10_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-17 17:10_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-17 17:10_

---

_Label `bug` added by @AlexWaygood on 2025-08-17 17:10_

---

_Label `ty` added by @AlexWaygood on 2025-08-17 17:10_

---

_Comment by @github-actions[bot] on 2025-08-17 17:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-19 10:29:21.750235491 +0000
+++ new-output.txt	2025-08-19 10:29:24.220254034 +0000
@@ -737,7 +737,6 @@
 overloads_evaluation.py:322:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
-protocols_definition.py:79:1: error[invalid-assignment] Object of type `Concrete` is not assignable to `Template`
 protocols_definition.py:114:1: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
 protocols_definition.py:115:1: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
 protocols_definition.py:116:1: error[invalid-assignment] Object of type `Concrete2_Bad3` is not assignable to `Template2`
@@ -860,5 +859,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 861 diagnostics
+Found 860 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-17 17:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- rich/layout.py:113:46: error[invalid-argument-type] Argument to function `ratio_resolve` is incorrect: Expected `Sequence[Edge]`, found `Sequence[Layout]`
- rich/layout.py:133:48: error[invalid-argument-type] Argument to function `ratio_resolve` is incorrect: Expected `Sequence[Edge]`, found `Sequence[Layout]`
- Found 312 diagnostics
+ Found 310 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:670 on 2025-08-19 08:47_

The polarity of these tests has changed. They previously asserted that `Foo` was not a a subtype of `HasXWithDefault`. Indeed, `Foo` does not have an `x` attribute with a default, so if the current tests reflect the intended behavior, can we add a comment why this is correct (is the fact that the protocol member has a default value irrelevant?)?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/protocol_class.rs`:570 on 2025-08-19 08:57_

We do this sort of thing a lot in ty's code base: iterating over both bindings and declarations separately, hoping that there is a 1:1 relationship. This is not always the case. For example:
```py
class D: # maybe a protocol, maybe a dataclass with fields, …
    if condition:
        attr: int
    else:
        attr = ""
```

This is unlikely to be a problem for protocols in particular, I think. And it's certainly not something specific to this PR. But I think it would be very beneficial in a lot of places if we could somehow iterate over all *definitions* and then get the declared type (for `attr: int`), the inferred type (for `attr = ""`), or both (for `attr: int = 0`).

Here, it would eliminate the mutation of the entry. And the bug would probably not have happened in the first place if the iterator would yield either a `DeclaredType { ty }`, an `InferredType { ty }`, or `Both { declared_ty, inferred_ty }`. See https://github.com/astral-sh/ruff/pull/19756 for a somewhat related problem that I fixed a few weeks ago.

---

_@sharkdp approved on 2025-08-19 09:05_

Thank you!

---

_@AlexWaygood reviewed on 2025-08-19 10:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:670 on 2025-08-19 10:14_

Yeah, I keep on changing my mind about this, and need to make a decision 😆 thanks for calling it out.

As the conformance-suite diff shows, the conformance suite tests include an example where a class with implicit instance attributes is considered a subtype of a protocol where the member is bound in the class body. I think you can make that sound, but only if you say that the instance attribute is _not_ present on the meta-protocol (`type[HasXWithDefault]`), which would obviously imply that we'd do attribute lookup differently for `type[]` Protocol types than we would for `type[]` nominal types.

I was actually confused about why the conformance suite diff changed, though, and you made me look again and realise that this PR was fixing _another_ bug. We were inferring this protocol as having an `x: Literal[0]` member rather than an `x: int` member:

```py
class P(Protocol):
    x: int = 0
```

I'll add a comment by this test, and add another test that explicitly asserts that we infer the correct interface for members that have default values.

---

_Merged by @AlexWaygood on 2025-08-19 10:31_

---

_Closed by @AlexWaygood on 2025-08-19 10:31_

---

_Branch deleted on 2025-08-19 10:31_

---

_@carljm reviewed on 2025-08-19 15:13_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/protocol_class.rs`:570 on 2025-08-19 15:13_

Created https://github.com/astral-sh/ty/issues/1051 from this comment

---
