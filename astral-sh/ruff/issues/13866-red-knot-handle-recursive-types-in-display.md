---
number: 13866
title: "[red-knot] Handle recursive types in `Display`"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2024-10-21T19:21:10Z
updated_at: 2024-12-13T09:36:32Z
url: https://github.com/astral-sh/ruff/issues/13866
synced_at: 2026-01-10T01:22:54Z
---

# [red-knot] Handle recursive types in `Display`

---

_Issue opened by @MichaReiser on 2024-10-21 19:21_

I haven't verified this but I'm 99% sure that our `Type::Display` implementation stack overflows if there's a recursive type. 

We should verify that `Type::Display` can handle recursive types.

---

_Label `red-knot` added by @MichaReiser on 2024-10-21 19:21_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:22_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-12-12 16:00_

---

_Comment by @MichaReiser on 2024-12-13 09:36_

I haven't been able to come up with cases where a user can construct a recursive type that then results in an infinite expansion during display. E.g. it would have to be a `Union` that contains itself as an element. I don't think it's possible for a user to define such a type and we should never infer such a type (fixpoint should give up after a few iterations). 

For future reference https://github.com/astral-sh/ruff/pull/14940

---

_Closed by @MichaReiser on 2024-12-13 09:36_

---
