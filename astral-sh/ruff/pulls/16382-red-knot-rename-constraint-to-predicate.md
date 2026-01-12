```yaml
number: 16382
title: "[red-knot] Rename constraint to predicate"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/predicates
created_at: 2025-02-25T19:41:33Z
updated_at: 2025-02-26T00:18:05Z
url: https://github.com/astral-sh/ruff/pull/16382
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Rename constraint to predicate

---

_@dcreager_

In https://github.com/astral-sh/ruff/pull/16306#discussion_r1966290700, @carljm pointed out that #16306 introduced a terminology problem, with too many things called a "constraint".  This is a follow-up PR that renames `Constraint` to `Predicate` to hopefully clear things up a bit.  So now we have that:

- a _predicate_ is a Python expression that might influence type inference
- a _narrowing constraint_ is a list of predicates that constraint the type of a binding that is visible at a use
- a _visibility constraint_ is a ternary formula of predicates that define whether a binding is visible or a statement is reachable

This is a pure renaming, with no behavioral changes.

---

_Label `internal` added by @dcreager on 2025-02-25 19:41_

---

_Label `red-knot` added by @dcreager on 2025-02-25 19:41_

---

_Review requested from @carljm by @dcreager on 2025-02-25 19:41_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-25 19:41_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-25 19:41_

---

_Review requested from @sharkdp by @dcreager on 2025-02-25 19:41_

---

_Merged by @dcreager on 2025-02-25 19:52_

---

_Closed by @dcreager on 2025-02-25 19:52_

---

_Branch deleted on 2025-02-25 19:52_

---

_Comment by @carljm on 2025-02-26 00:18_

Thank you!

---
