```yaml
number: 17617
title: "[red-knot] Create generic context for generic classes lazily"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/lazy-generic-class
created_at: 2025-04-24T22:10:08Z
updated_at: 2025-04-25T18:10:05Z
url: https://github.com/astral-sh/ruff/pull/17617
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Create generic context for generic classes lazily

---

_@dcreager_

As discussed today, this is needed to handle legacy generic classes without having to infer the types of the class's explicit bases eagerly at class construction time. Pulling this out into a separate PR so there's a smaller diff to review.

This also makes our representation of generic classes and functions more consistent — before, we had separate Rust types and enum variants for generic/non-generic classes, but a single type for generic functions. Now we each a single (respective) type for each.

There were very few places we were differentiation between generic and non-generic _class literals_, and these are handled now by calling the (salsa cached) `generic_context` _accessor function_.

Note that _`ClassType`_ is still an enum with distinct variants for non-generic classes and specialized generic classes.

---

_Label `red-knot` added by @dcreager on 2025-04-24 22:10_

---

_Review requested from @carljm by @dcreager on 2025-04-24 22:10_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-24 22:10_

---

_Review requested from @sharkdp by @dcreager on 2025-04-24 22:10_

---

_Comment by @github-actions[bot] on 2025-04-24 22:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-24 22:24_

Looks great!

---

_@MichaReiser approved on 2025-04-25 06:20_

This looks nice! 

It does lead to a small 3% perf regression on incremental benchmarks. I suspect it is because it now requires two salsa ingredients for every class (generic context and the class itself). 

---

_Merged by @dcreager on 2025-04-25 18:10_

---

_Closed by @dcreager on 2025-04-25 18:10_

---

_Branch deleted on 2025-04-25 18:10_

---
