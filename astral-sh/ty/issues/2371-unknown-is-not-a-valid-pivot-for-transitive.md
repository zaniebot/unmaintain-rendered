---
number: 2371
title: "`Unknown` is not a valid pivot for transitive constraints in a constraint set"
type: issue
state: closed
author: dcreager
labels:
  - generics
assignees: []
created_at: 2026-01-06T21:11:13Z
updated_at: 2026-01-08T14:31:57Z
url: https://github.com/astral-sh/ty/issues/2371
synced_at: 2026-01-10T01:51:14Z
---

# `Unknown` is not a valid pivot for transitive constraints in a constraint set

---

_Issue opened by @dcreager on 2026-01-06 21:11_

As part of building up constraint sets, we produce a "sequent map" (aka a Horn clause database) describing how all of the individual constraints in the constraint set, and any derived constraints, relate to each other. Part of this is building up transitive relationships. For instance, if we know that `S ≤ int` and `int ≤ T`, we can infer that `S ≤ T`.

In https://github.com/astral-sh/ruff/pull/22411, we started seeing some false positive diagnostics, whose root cause seems to be sequents of the form `S ≤ Any ∧ Any ≤ T → S ≤ T`. That is, exactly the same as above, but with `Any` as the "pivot" instead of `int`. But that's not valid, since there's no guarantee that the two `Any`s will materialize to the same type. In other words, we should be performing a _subtyping_ check during sequent map constructing, not an _assignability_ check.

---

_Assigned to @dcreager by @dcreager on 2026-01-06 21:11_

---

_Label `generics` added by @AlexWaygood on 2026-01-06 21:12_

---

_Added to milestone `Stable` by @carljm on 2026-01-06 21:21_

---

_Closed by @dcreager on 2026-01-08 14:31_

---
