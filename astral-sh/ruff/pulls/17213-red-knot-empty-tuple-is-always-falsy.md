```yaml
number: 17213
title: "[red-knot] Empty tuple is always-falsy"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-empty-tuple-always-falsy
created_at: 2025-04-04T19:22:24Z
updated_at: 2025-04-04T20:00:31Z
url: https://github.com/astral-sh/ruff/pull/17213
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Empty tuple is always-falsy

---

_@sharkdp_

## Summary

Fix assignability of `tuple[()]` to `AlwaysFalsy`.

closes astral-sh/ruff#17202 

## Test Plan

Ran the property tests for a while

---

_Label `red-knot` added by @sharkdp on 2025-04-04 19:22_

---

_Review requested from @carljm by @sharkdp on 2025-04-04 19:22_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-04 19:22_

---

_Review requested from @dcreager by @sharkdp on 2025-04-04 19:22_

---

_@sharkdp reviewed on 2025-04-04 19:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1080 on 2025-04-04 19:23_

The difference is that we now allow a fall-through to the catch-all "self is subtype of target" branch.

---

_Comment by @github-actions[bot] on 2025-04-04 19:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @carljm on 2025-04-04 19:25_

I have a vague memory of intentionally making the opposite decision here.

I think the key question we need to ensure we are answering consistently is, do instances of subclasses of the built-in `tuple` type also inhabit our `Type::Tuple` types? My memory is that in other places we assume they can, but I would have to double-check. If they do, then we cannot say that the empty `Type::Tuple` is always-falsy, since the empty instance of a subclass of `tuple` could be truthy.

The potential problem with saying they do not, is that we do need to allow code like this:

```py
def f(t: tuple[int, str]): ...

class MyTuple(tuple[int, str]): pass

f(MyTuple((1, "foo")))
```

In other words, if we say they do not, then our `Type::Tuple` can no longer correspond to the semantics of a `tuple` annotation.

---

_@AlexWaygood approved on 2025-04-04 19:25_

---

_Comment by @AlexWaygood on 2025-04-04 19:30_

> I have a vague memory of intentionally making the opposite decision here.

Ah yes, https://github.com/astral-sh/ty/issues/215 is still unresolved... I have a long reply in my head I've been meaning to write up there for a while :-) maybe I should set an explicit goal for myself to do that on Monday...

Anyway, I think the change to the code here is correct even if we disagree on the assertion being made in the mdtest!

---

_Comment by @carljm on 2025-04-04 19:38_

Yes, I think the code change is correct, but we should add a TODO comment above the assertions (and maybe also above similar assertions for subtyping, if we have those already?) that we either need to special-case enforcement of Liskov on `__bool__` and `__len__` for tuple subclasses, or stop assuming that empty tuples are falsy and non-empty tuples are truthy.

---

_Merged by @sharkdp on 2025-04-04 20:00_

---

_Closed by @sharkdp on 2025-04-04 20:00_

---

_Branch deleted on 2025-04-04 20:00_

---
