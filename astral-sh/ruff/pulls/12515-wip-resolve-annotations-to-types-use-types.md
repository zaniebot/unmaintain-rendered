```yaml
number: 12515
title: " WIP: Resolve annotations to types; use `types.ModuleType` for unresolved global attributes"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: moduletype-attrs
created_at: 2024-07-25T17:36:18Z
updated_at: 2024-10-24T09:58:02Z
url: https://github.com/astral-sh/ruff/pull/12515
synced_at: 2026-01-12T15:55:41Z
```

#  WIP: Resolve annotations to types; use `types.ModuleType` for unresolved global attributes

---

_@AlexWaygood_

(Don't look at this yet, just something I was pairing on with @carljm just now. Very much a work in progress right now!)

---

_Label `red-knot` added by @AlexWaygood on 2024-07-25 17:36_

---

_Renamed from "Add a failing test" to " WIP: Resolve annotations to types; use `types.ModuleType` for unresolved global attributes" by @AlexWaygood on 2024-07-25 17:43_

---

_Comment by @carljm on 2024-07-26 15:59_

@AlexWaygood one thing I realized yesterday in thinking about this more is that I'm not sure lazily resolving class bases will work well, because if you inherit from `Generic` or `Protocol` we will I think want to record the class differently (i.e. as a different variant of the `Type` enum). If that's true, then I think resolving the `str` type at all is blocked on Salsa cycle handling (barring some kind of ugly temporary hack like changing typeshed or special-casing lookup of the `str` type.)

But I could be wrong; maybe we can record generic classes and protocols all as `Type::Class`, with internal differentiation in the `ClassType` struct. If so, then resolving bases lazily might be an option (but I think we'd probably end up resolving the bases on almost any use of the type.) It might be hard to come to a clear conclusion here without going down the path of designing our handling of protocols and generics, which I hadn't planned to tackle yet.

---

_Comment by @AlexWaygood on 2024-07-26 16:07_

FWIW, mypy has the same representation for protocol definitions as it does for class definitions — it has has a boolean flag that you can query to tell you whether it's a protocol or not: https://github.com/python/mypy/blob/e67decb91ea5aacbb1e126463b02d78ded680a90/mypy/nodes.py#L2967

(But that obviously doesn't mean that that's the best representation for us to use!)

I'm AFK until Monday, but we could also just change the test to use `__spec__` rather than `__name__` for now, since `__spec__` is not a `str`.

---

_Comment by @AlexWaygood on 2024-10-24 09:58_

Something very strange happened to the diff here... seems like this PR is lost beyond all hope! Better to start afresh.

---

_Closed by @AlexWaygood on 2024-10-24 09:58_

---

_Branch deleted on 2024-10-24 09:58_

---
