```yaml
number: 14104
title: "`types.rs`: remove unused `is_stdlib_symbol` methods"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/types-cleanup
created_at: 2024-11-05T11:57:45Z
updated_at: 2024-11-05T12:46:19Z
url: https://github.com/astral-sh/ruff/pull/14104
synced_at: 2026-01-12T15:55:46Z
```

# `types.rs`: remove unused `is_stdlib_symbol` methods

---

_@AlexWaygood_

## Summary

These methods are now unused: we now instead use the `KnownFunction`, `KnownClass` and `KnownInstance` enums to track whether a `FunctionLiteral`, `ClassLiteral` or `Instance` type represents a specific symbol that we care about. I also don't believe that these methods are particularly efficient. Let's get rid of them; we can always add them back later if we rediscover a need for them.

## Test Plan

`cargo test -p red_knot_python_semantic`

---

_Label `red-knot` added by @AlexWaygood on 2024-11-05 11:57_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-05 11:57_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-05 11:57_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-05 11:57_

---

_Comment by @github-actions[bot] on 2024-11-05 12:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-11-05 12:44_

I'm now inclined to remove `name` and instead store the `Symbolid`.... 

---

_Merged by @AlexWaygood on 2024-11-05 12:46_

---

_Closed by @AlexWaygood on 2024-11-05 12:46_

---

_Branch deleted on 2024-11-05 12:46_

---
