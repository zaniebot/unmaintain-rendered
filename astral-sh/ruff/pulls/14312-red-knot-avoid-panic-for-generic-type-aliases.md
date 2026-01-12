```yaml
number: 14312
title: "[red-knot] Avoid panic for generic type aliases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14307-patch
created_at: 2024-11-13T09:45:29Z
updated_at: 2024-11-13T15:01:18Z
url: https://github.com/astral-sh/ruff/pull/14312
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Avoid panic for generic type aliases

---

_@sharkdp_

## Summary

This avoids a panic inside `TypeInferenceBuilder::infer_type_parameters` when encountering generic type aliases:
```py
type ListOrSet[T] = list[T] | set[T]
```

I'm not sure if this is the right call. To fix this properly, I believe we would have to treat type aliases as being [their own annotation scope](https://docs.python.org/3/reference/compound_stmts.html#generic-type-aliases). The left hand side is a definition for the type parameter `T` which is being used in the special annotation scope on the right hand side. Similar to how it works for generic functions and classes. This is something I've started to look into — but it seems to require larger-scale changes and I'm not sure if this is the right time to work on them.

closes #14307

## Test Plan

Added new example to the corpus.


---

_Label `red-knot` added by @sharkdp on 2024-11-13 09:45_

---

_Review requested from @carljm by @sharkdp on 2024-11-13 09:45_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-13 09:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-13 09:45_

---

_Comment by @github-actions[bot] on 2024-11-13 09:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-11-13 11:26_

> I'm not sure if this is the right call.

I'm not sure either 😆 But maybe it's acceptable for now to avoid a panic. Not sure -- let's wait for @carljm to wake up

---

_Comment by @sharkdp on 2024-11-13 13:55_

Just for reference: my — so far unsuccessful — attempt at solving this by adding a new scope for type aliases: https://github.com/astral-sh/ruff/compare/main...david/fix-14307

---

_@carljm approved on 2024-11-13 14:29_

It is true that generic type aliases also have a type-parameters scope, like generic functions and classes, and we should handle it similarly.

I think this TODO is fine for now, though I'm a bit surprised it doesn't cause us to have expressions missing in our test that walks the AST and pulls a type for every expression.

I didn't have PEP695 type alias support on the list for 2024, but we'll definitely need it and it's a bit tricky, so I'd be weakly inclined to go ahead with your PR to fix this properly, since it looks like you already made a lot of progress on it, and the direction looks right. I will need to check it out locally if it's still failing and you need help understanding why.

---

_Comment by @sharkdp on 2024-11-13 14:59_

Ok, thanks. I'm merging this for now to close / clean-up a few tickets. And I can open a new ticket for proper type alias support.

---

_Merged by @sharkdp on 2024-11-13 15:01_

---

_Closed by @sharkdp on 2024-11-13 15:01_

---

_Branch deleted on 2024-11-13 15:01_

---
