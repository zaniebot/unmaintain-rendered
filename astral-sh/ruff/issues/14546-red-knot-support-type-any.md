```yaml
number: 14546
title: "[red-knot] support type[Any]"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-22T22:31:44Z
updated_at: 2024-12-10T21:12:39Z
url: https://github.com/astral-sh/ruff/issues/14546
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] support type[Any]

---

_Issue opened by @carljm on 2024-11-22 22:31_

The Python type system supports the `type[Any]` type as a special case of [the `type[...]` special form](https://typing.readthedocs.io/en/latest/spec/special-types.html#type). This is a gradual type representing some unknown class and all of its subclasses.

Once we have fixed https://github.com/astral-sh/ruff/issues/14544, we should also support `type[Any]`. This will require updating our representation of `SubclassOfType` to an enum, so it can store either a `Class` or just represent `type[Any]`. This will also require checking each place we handle `Type::SubclassOf` and updating it to handle the gradual variant correctly.

We should also infer `type[Any]` from an annotation that just uses bare `type`, e.g. `x: type = ...`.


---

_Label `red-knot` added by @carljm on 2024-11-22 22:31_

---

_Assigned to @carljm by @carljm on 2024-11-22 22:31_

---

_Comment by @AlexWaygood on 2024-11-23 10:43_

I suspect the enum we want here will have the same variants as this one:

https://github.com/astral-sh/ruff/blob/07d13c6b4a9acdfac3fbce314ee50685d0e8df0d/crates/red_knot_python_semantic/src/types/mro.rs#L303

I don't know whether it's worth trying to reuse (and rename?) that one or create another one with the same variants

---

_Comment by @InSyncWithFoo on 2024-12-08 02:54_

This seems simple enough. I'll have a PR ready by tomorrow.

---

_Comment by @carljm on 2024-12-08 02:57_

> This seems simple enough. I'll have a PR ready by tomorrow.

Appreciate the interest in contributing! This issue (and a few others) are assigned to me because they are on the ramp up plan for a new Astral team member. If there isn't an unassigned issue that looks interesting to you, I can create some additional ones soon!

---

_Comment by @InSyncWithFoo on 2024-12-08 03:10_

> This issue (and a few others) are assigned to me because they are on the ramp up plan for a new Astral team member.

Is that so? The branch is at 90% already, but I guess I'll have to drop it then.

---

_Assigned to @dcreager by @dcreager on 2024-12-09 15:13_

---

_Unassigned @carljm by @dcreager on 2024-12-09 15:13_

---

_Comment by @dcreager on 2024-12-10 21:11_

> We should also infer `type[Any]` from an annotation that just uses bare `type`, e.g. `x: type = ...`.

Surfacing from https://github.com/astral-sh/ruff/pull/14876#discussion_r1878322282 that we have decided to interpret bare `type` as `type[object]`, not `type[Any]`.

---

_Closed by @dcreager on 2024-12-10 21:12_

---

_Closed by @dcreager on 2024-12-10 21:12_

---
