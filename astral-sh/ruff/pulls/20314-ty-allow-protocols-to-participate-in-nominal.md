```yaml
number: 20314
title: "[ty] Allow protocols to participate in nominal subtyping as well as structural subtyping"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-nominal
created_at: 2025-09-09T12:16:28Z
updated_at: 2025-09-10T11:06:55Z
url: https://github.com/astral-sh/ruff/pull/20314
synced_at: 2026-01-12T15:56:59Z
```

# [ty] Allow protocols to participate in nominal subtyping as well as structural subtyping

---

_@AlexWaygood_

## Summary

(Stacked on top of https://github.com/astral-sh/ruff/pull/20284; review that PR first.)

Currently our implementation of subtyping and assignability checks against protocols only checks whether a given type is _structurally_ a subtype of/assignable to a given protocol. Other type checkers, however, allow protocols to participate in nominal subtyping _as well as_ structural subtyping. The most common example where this comes up is that all other type checkers consider `str` to be a subtype of `Container[str]` even though `str.__contains__` is less permissive in the types it accepts than `Container.__contains__`; the reason they consider it a subtype nonetheless is that `str` has `Container[str]` in its MRO. We currently also consider `str` to be a subtype of `Container[str]`, but this is because we still do not look at the types of method members when doing subtyping/assignability checks for protocols; an early version of https://github.com/astral-sh/ruff/pull/20165 revealed many ecosystem false positives due to our failure to understand `str` as being a subtype of `Container[str]`.

This PR therefore updates our subtyping/assignability logic for protocols so that, for a given class-based protocol, we also check whether a given other type could be considered a nominal subtype of that protocol before concluding that there exists no subtype relation between the other type and the protocol.

The implication of this is that one of our property tests must be removed: `all_type_assignable_to_iterable_are_iterable`. Just because a type can be assignable to `Iterable` does not mean it is necessarily iterable -- it could be a Liskov-uncompliant subclass of `Iterable`! I don't think we have any Liskov-uncompliant classes in our property-test generation right now, but this still seems like a potential cause for confusion if we added one (deliberately or not) in the future. As such, I've removed the property test as part of this PR.

## Test Plan

Mdtests added.


---

_Label `ty` added by @AlexWaygood on 2025-09-09 12:16_

---

_Comment by @github-actions[bot] on 2025-09-09 12:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-09 12:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-09-09 12:51_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-09 12:51_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-09 12:51_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-09 12:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/property_tests.rs`:327 on 2025-09-09 21:28_

I'm not convinced by the rationale given in the PR description for removing this property test. I think there are many type properties that we want to verify for valid types, that can be invalidated by an invalid type (e.g. a type that violates Liskov), and we probably should never introduce such types into our type generation for the property tests. Or if we do, we should still keep some property tests with a pre-condition that they only apply to valid types.

Especially if there's no _current_ problem, I would not remove this property test. If anything, I'd just add a note to its docstring that it is not true for types which explicitly inherit `Iterable` but violate LSP on its interface.

---

_@carljm approved on 2025-09-09 21:31_

👍 

---

_@AlexWaygood reviewed on 2025-09-09 21:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/property_tests.rs`:327 on 2025-09-09 21:38_

hmm, I suppose we also have property tests that guarantee that subtyping is intransitive, and they would also be broken by Liskov-uncompliant classes interacting with protocols.

I still worry a bit that this could cause confusion for us at some point... there are a lot of Liskov-uncompliant classes in the standard library! Let's hope we're vigilant about keeping them out of our property tests...

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1070 on 2025-09-09 21:39_

I should add an mdtest here for calling `tuple()` on a Liskov-uncompliant subclass of `Iterable`

---

_@AlexWaygood reviewed on 2025-09-09 21:39_

---

_Merged by @AlexWaygood on 2025-09-10 11:05_

---

_Closed by @AlexWaygood on 2025-09-10 11:05_

---

_Branch deleted on 2025-09-10 11:05_

---
