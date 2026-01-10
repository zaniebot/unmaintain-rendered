```yaml
number: 202
title: Disjointness of complements
type: issue
state: open
author: sharkdp
labels:
  - type properties
  - set-theoretic types
assignees: []
created_at: 2025-02-10T20:13:32Z
updated_at: 2025-11-18T16:10:22Z
url: https://github.com/astral-sh/ty/issues/202
synced_at: 2026-01-10T02:06:24Z
```

# Disjointness of complements

---

_Issue opened by @sharkdp on 2025-02-10 20:13_

### Description

The complement `~T` of a type is disjoint from the type `T` itself. In fact, it is disjoint from all subtypes of `S <: T`.

It is rather straightforward to implement this. The problem I am facing is that this leads to new property test failures, because it breaks the symmetry of the is-disjoint-from relation when done naively.

The reason for that are pairs like `T & Any` and `~T`. In one direction, if we check if `T & Any` is disjoint from `~T`, we start by checking if any positive element of the intersection `T & Any` is disjoint from `~T`. And since `T` is disjoint from `~T` according to this new check, we return `true`, as we should.

But when we check if `~T` is disjoint from `T & Any`, we have no positive elements in the left-hand side intersection. The only remaining check is therefore the new check which tries if `T & Any` is a subtype of `T`. This is not the case, because `T & Any` is not fully-static. But if feels like this "is-subtype-of" relation in the check could be replaced by a subtyping-relation on gradual types that would check if *all* materializations of `S` are subtypes of all materializations of `T` (which is different from the consistent-subtype relation on gradual types).

---

For context, this came up when trying to write the following test that intends to demonstrate that `AlwaysTruthy`/`AlwaysFalys` are disjoint from `AmbiguousTruthiness`
```py
type Truthy = Not[AlwaysFalsy]
type Falsy = Not[AlwaysTruthy]

type AmbiguousTruthiness = Intersection[Truthy, Falsy]

static_assert(is_disjoint_from(AlwaysTruthy, AmbiguousTruthiness))
static_assert(is_disjoint_from(AlwaysFalsy, AmbiguousTruthiness))
```

![image](https://github.com/user-attachments/assets/55f4c7b2-86d3-4aac-847d-65ac009ebafe)

---

_Assigned to @sharkdp by @sharkdp on 2025-02-10 20:13_

---

_Unassigned @sharkdp by @sharkdp on 2025-04-28 08:18_

---

_Renamed from "[red-knot] Disjointness of complements" to "Disjointness of complements" by @MichaReiser on 2025-05-07 15:26_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:19_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:20_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
