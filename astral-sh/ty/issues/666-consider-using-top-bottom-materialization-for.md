```yaml
number: 666
title: Consider using top / bottom materialization for assignability check
type: issue
state: open
author: dhruvmanila
labels:
  - needs-design
  - type properties
assignees: []
created_at: 2025-06-17T05:12:00Z
updated_at: 2025-11-18T16:10:33Z
url: https://github.com/astral-sh/ty/issues/666
synced_at: 2026-01-12T15:54:23Z
```

# Consider using top / bottom materialization for assignability check

---

_@dhruvmanila_

The concept of top and bottom materialization of a type was added in https://github.com/astral-sh/ruff/pull/18594.

@carljm mentioned that this could potentially be used to perform assignability, especially when gradual types are involved. Formally, if we want to check whether `T` is assignable to `U`, then we could check whether `T'`, the bottom materialization (or lower bound materialization) of `T`, is a subtype of `U'`, the top materialization (or upper bound materialization) of `U`.

The main benefit here is that this would eliminate any special handling of gradual types in the subtyping check and could eliminate any potential bugs that might exists in the current assignability implementation.

This issue is to keep track of this potential change if we decide to do it.

This would still require fulfilling the TODOs that are currently present in the implementation of type materialization in `Type::materialize`.

---

_Label `needs-design` added by @dhruvmanila on 2025-06-17 05:12_

---

_Label `type properties` added by @dhruvmanila on 2025-06-17 05:12_

---

_Comment by @jelle-openai on 2025-06-17 16:33_

From reading your description of the concept of a "top materialization", I think this would be incorrect because of the issue I discovered in https://github.com/python/typing/issues/2027.

---

_Comment by @carljm on 2025-06-17 21:51_

I think to be correct, it requires that the top materialization of e.g. `list[Any]` properly be the type `list[*]` (that is, the type including all lists, no matter their contained type); this is already mentioned in our materialization implementation as a TODO. So this issue is dependent on having some representation for that type.

Currently we approximate the top materialization by materializing `list[Any]` to `list[T]`, where `T` is an unresolved type variable. This adequately fulfills our needs for the current use of materialization in overload resolution, where we simply need `list[T]` to be assignable only to `list[Any]` and no other list type (that is, for our current purposes we only need to care about what the top materialization is assignable to, not what is assignable to the top materialization). But you're right that fixing the top materialization so it works correctly in both directions is a pre-requisite to moving forward with this issue.

This type `list[*]` is really just the "top" extension of the rule you mention in https://github.com/python/typing/issues/2027 (that if `A'` and `A''` are both materializations of `A`, `A' | A''` must be as well), since it is the union of all possible "immediate" materializations of `list[Any]`.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:09_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
