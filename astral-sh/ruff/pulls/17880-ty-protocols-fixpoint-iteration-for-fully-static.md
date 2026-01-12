```yaml
number: 17880
title: "[ty] Protocols: Fixpoint iteration for fully-static check"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-17861
created_at: 2025-05-06T07:21:17Z
updated_at: 2025-05-07T06:55:23Z
url: https://github.com/astral-sh/ruff/pull/17880
synced_at: 2026-01-12T15:56:07Z
```

# [ty] Protocols: Fixpoint iteration for fully-static check

---

_@sharkdp_

## Summary

A recursive protocol like the following would previously lead to stack overflows when attempting to create the union type for the `P | None` member, because `UnionBuilder` checks if element types are fully static, and the fully-static check on `P` would in turn list all members and check whether all of them were fully static, leading to a cycle.

```py
from __future__ import annotations

from typing import Protocol

class P(Protocol):
    parent: P | None
```

Here, we make the fully-static check on protocols a salsa query and add fixpoint iteration, starting with `true` as the initial value (assume that the recursive protocol is fully-static). If the recursive protocol has any non-fully-static members, we still return `false` when re-executing the query (see newly added tests).

closes #17861

## Test Plan

Added regression test

---

_Label `ty` added by @sharkdp on 2025-05-06 07:21_

---

_Comment by @github-actions[bot] on 2025-05-06 07:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-06 07:38_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/protocol_class.rs`:115 on 2025-05-06 07:38_

Not sure if there is a way to avoid cloning here, since `_members(db)` only gives us shared access to the `BTreeMap`.

---

_Marked ready for review by @sharkdp on 2025-05-06 07:38_

---

_Review requested from @carljm by @sharkdp on 2025-05-06 07:38_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-06 07:38_

---

_Review requested from @dcreager by @sharkdp on 2025-05-06 07:38_

---

_Comment by @AlexWaygood on 2025-05-06 07:41_

This implies that I might also need to add fixpoint for any other operation that involves probing the types of the protocols members, such as assignability and disjointness checks. That feels slightly painful, but so it goes, I guess!

---

_@AlexWaygood reviewed on 2025-05-06 07:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:115 on 2025-05-06 07:48_

I think you could change `ProtocolMemberData::normalized()` to take `&self` rather than `self`. It probably wouldn't improve perf, but it would avoid the need for one of the explicit `.clone()` calls, which maybe makes us feel slightly better about life?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1 on 2025-05-06 07:53_

Can we create a new `## Recursive protocols` section of this test suite (that includes the assertions you're adding here, and that also includes the tests I added earlier at https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/protocols.md#regression-test-narrowing-with-self-referential-protocols )?. I think it makes sense for all recursive-protocol tests to be together.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1382 on 2025-05-06 07:57_

These assertions should help prevent me from adding overflows when I add better assignability/subtyping logic for protocols in the future:

```suggestion
static_assert(not is_fully_static(RecursiveNonFullyStatic))
static_assert(is_assignable_to(RecursiveFullyStatic, RecursiveNonFullyStatic))
static_assert(is_assignable_to(RecursiveNonFullyStatic, RecursiveFullyStatic))
static_assert(not is_subtype_of(RecursiveFullyStatic, RecursiveNonFullyStatic))
static_assert(not is_subtype_of(RecursiveNonFullyStatic, RecursiveFullyStatic))
```

I'd also be interested in this test, not sure if it passes or not yet (could add it with a TODO if it doesn't):

```py
class AlsoRecursiveFullyStatic(Protocol):
    parent: AlsoRecursiveFullyStatic | None
    x: int

static_assert(is_equivalent_to(AlsoRecursiveFullyStatic, RecursiveFullyStatic))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:305 on 2025-05-06 07:59_

The inner struct becomes redundant with this refactor. You can get rid of it and have `SynthesizedProtocolType` just be

```rs
#[derive(Copy, Clone, Debug, Eq, PartialEq, Hash, salsa::Update, PartialOrd, Ord)]
pub(in crate::types) struct SynthesizedProtocolType<'db>(ProtocolInterface<'db>);

impl<'db> SynthesizedProtocolType<'db> {
    pub(super) fn new(db: &'db dyn Db, interface: ProtocolInterface<'db>) -> Self {
        Self(interface.normalized(db))
    }

    pub(in crate::types) fn interface(self) -> ProtocolInterface<'db> {
        self.0
    }
}
```


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:122 on 2025-05-06 08:00_

:/ The idea was that this would be an implementation detail that wouldn't be exposed outside of this module at all. I guess this is necessary because of Salsa visibility rules? It doesn't matter too much, I guess :-)

---

_@AlexWaygood approved on 2025-05-06 08:00_

Thank you!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/protocol_class.rs`:122 on 2025-05-06 11:00_

Yeah, this is unfortunate. Not sure if there is a way to control the visibility...

---

_@sharkdp reviewed on 2025-05-06 11:00_

---

_@sharkdp reviewed on 2025-05-06 11:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/instance.rs`:305 on 2025-05-06 11:01_

Makes sense, I wasn't sure if it was a necessary abstraction layer or just there for `Copy`-purposes or similar.

---

_@sharkdp reviewed on 2025-05-06 11:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1382 on 2025-05-06 11:01_

So... both `is_assignable_to` as well as `is_equivalent_to` checks are also leading to stack overflows. I'll look into it.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:305 on 2025-05-06 11:06_

It was basically just there to make it impossible to construct `SynthesizedProtocolType` without calling `new()`. If I made `SynthesizedProtocolType` itself interned, I wouldn't have been able to give it a custom `new()` method, so I made it a wrapper around a Salsa-interned inner type. I wanted to enforce that it could only be constructed using the `new()` method so that I could enforce the invariant that the inner interface of a `SynthesizedProtocolType` could only ever be a normalized interface.

---

_@AlexWaygood reviewed on 2025-05-06 11:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1382 on 2025-05-07 06:31_

I have not yet found a solution to this problem. I added TODO comments for now. I am going to merge this PR, as it solves the originally reported problem, and open a new ticket for the remaining tasks (including a note that the cycle handling for `is_fully_static` might be replaced with a more general solution).

---

_@sharkdp reviewed on 2025-05-07 06:31_

---

_@AlexWaygood reviewed on 2025-05-07 06:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1382 on 2025-05-07 06:39_

SGTM, thank you!

---

_Merged by @sharkdp on 2025-05-07 06:55_

---

_Closed by @sharkdp on 2025-05-07 06:55_

---

_Branch deleted on 2025-05-07 06:55_

---
