```yaml
number: 17929
title: "[ty] Recursive protocols"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/recursive-protocols
created_at: 2025-05-07T19:34:35Z
updated_at: 2025-05-09T12:54:03Z
url: https://github.com/astral-sh/ruff/pull/17929
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Recursive protocols

---

_@sharkdp_

## Summary

Use a self-reference "marker" ~~and fixpoint iteration~~ to solve the stack overflow problems with recursive protocols. This is not pretty and somewhat tedious, but seems to work fine. Much better than all my fixpoint-iteration attempts anyway.

closes https://github.com/astral-sh/ty/issues/93

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-05-07 19:34_

---

_Comment by @github-actions[bot] on 2025-05-07 19:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-05-08 13:17_

---

_Review requested from @carljm by @sharkdp on 2025-05-08 13:17_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-08 13:17_

---

_Review requested from @dcreager by @sharkdp on 2025-05-08 13:17_

---

_Closed by @sharkdp on 2025-05-08 13:17_

---

_Reopened by @sharkdp on 2025-05-08 13:17_

---

_Comment by @sharkdp on 2025-05-08 13:19_

Moving this out of draft-mode to get a general review before I spend more time resolving all the minor TODOs.

---

_@AlexWaygood reviewed on 2025-05-08 13:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:652 on 2025-05-08 13:40_

could you add this test, with a TODO comment that it should pass? (It doesn't overflow, but the static assertion fails.) I can tackle it later.

```py
class Foo(Protocol):
    @property
    def x(self) -> "Foo": ...

class Bar(Protocol):
    @property
    def x(self) -> "Bar": ...

static_assert(is_equivalent_to(Foo, Bar))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1620 on 2025-05-08 13:47_

```suggestion
Make sure that we handle self-reference correctly, even if the self-reference appears deeply nested
within the type of a protocol member:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:119 on 2025-05-08 13:50_

```suggestion
        self.members(db).all(|member| member.ty.is_fully_static(db))
```

---

_@AlexWaygood reviewed on 2025-05-08 13:52_

This looks like an excellent solution to me!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1632 on 2025-05-08 14:15_

```suggestion
Make sure that we handle self-reference correctly, even if the self-reference appears deeply nested
```

---

_@AlexWaygood reviewed on 2025-05-08 14:15_

---

_@sharkdp reviewed on 2025-05-08 14:20_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1632 on 2025-05-08 14:20_

mdformat, what are you doing...

---

_@MichaReiser reviewed on 2025-05-08 14:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1632 on 2025-05-08 14:38_

Someday I'll write a markdown formatter ;)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:634 on 2025-05-08 16:10_

Was just about to ask if we could re-use this technique for generics :smile: 

:+1: for it being a TODO

---

_@dcreager reviewed on 2025-05-08 16:11_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1674 on 2025-05-08 17:03_

These assertions are good, but it would be nice to also have some assertions that we actually understand these nested recursive types, e.g. that `instance_of_recursive.t[1][1]` is `Recursive`, etc.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:598 on 2025-05-08 17:05_

Should we be consistent about naming? `replace_self_reference` vs `replace_recursive_reference`

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:594 on 2025-05-08 17:08_

I suspect that in order to reuse this for generics, we might have to accept more possibilities here than just ClassLiteral?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/protocol_class.rs`:69 on 2025-05-08 17:20_

It seems like this approach has a significant limitation, in that it can handle direct self-reference, but it can't handle mutual recursion (because we can only represent a self-reference, not a "recursive reference to arbitrary something", which means we can't break a recursive loop involving more than one protocol.) And indeed, this still stack overflows on this branch:

```py
from __future__ import annotations

from typing import Protocol
from ty_extensions import is_fully_static, is_subtype_of, static_assert

class Foo(Protocol):
    x: Bar

class Bar(Protocol):
    x: Foo

static_assert(is_fully_static(Foo))
static_assert(is_fully_static(Bar))
```

---

_@carljm approved on 2025-05-08 17:24_

Thank you!!

This clearly addresses a lot of the practical cases we see, but it does still have a problem with mutual recursion of multiple different classes.

I think we could consider landing this anyway as an initial step, but I think we will need to figure out how to generalize it beyond simple self-reference.

---

_@sharkdp reviewed on 2025-05-08 17:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:634 on 2025-05-08 17:26_

The doc-comment of this function is probably more relevant? This is just the place where we would also have to look for references to the protocol class.

Instead of `ClassLiteral`, I would imagine that this function would maybe have to take `Type` as an argument if we want to handle self-referential type aliases like `type Tree = dict[str, Tree]`. Or maybe an enum (different things that we would want to replace).

---

_@sharkdp reviewed on 2025-05-08 18:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:594 on 2025-05-08 18:12_

Yes, see the other comment above in response to Doug (which I wrote after this was posted, I guess).

---

_Comment by @sharkdp on 2025-05-08 18:30_

After talking to @carljm we decided to merge this as an immediate solution to an observed real-world problem. I don't think it introduces any *wrong* behavior either. And as Carl pointed out, the tests are hopefully useful in any case.

That said, we also think that there are severe limitations here (see Carl's comment regarding mutually-recursive protocols). And it's also not clear if this is a generalizable solution that would also work for self-referential type aliases or generic classes. @dcreager: it's probably not useful to spend time right now trying to make this specific approach here work for generics.

What I will do instead after merging this is to take one step back and think about [the broader problem here](https://github.com/astral-sh/ty/issues/256), and do some research. Hopefully that leads to a general solution. I might not be able to spend the necessary amount of time on this before Tuesday. So if anyone else wants to work on this before then, feel free to do that (let me know).

---

_Merged by @sharkdp on 2025-05-09 12:54_

---

_Closed by @sharkdp on 2025-05-09 12:54_

---

_Branch deleted on 2025-05-09 12:54_

---
