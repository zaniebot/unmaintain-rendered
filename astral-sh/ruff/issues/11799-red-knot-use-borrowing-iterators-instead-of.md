```yaml
number: 11799
title: "[red-knot] use borrowing iterators instead of returning vecs for Union.elements, Intersection.{positive|negative}"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-07T16:38:02Z
updated_at: 2024-08-05T17:33:42Z
url: https://github.com/astral-sh/ruff/issues/11799
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] use borrowing iterators instead of returning vecs for Union.elements, Intersection.{positive|negative}

---

_@carljm_

See https://github.com/astral-sh/ruff/pull/11783#issuecomment-2155133526


---

_Label `red-knot` added by @carljm on 2024-06-07 16:38_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-06-07 16:39_

---

_Comment by @AlexWaygood on 2024-06-07 16:40_

(Deferred for now until the coming Salsa refactor)

---

_Comment by @carljm on 2024-08-05 17:33_

This was made obsolete in the Salsa refactor. At the moment we don't expose elements/positive/negative directly at all, just methods like `UnionType::contains`.

---

_Closed by @carljm on 2024-08-05 17:33_

---
