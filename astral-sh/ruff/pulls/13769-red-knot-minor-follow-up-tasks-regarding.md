```yaml
number: 13769
title: "[red knot] Minor follow-up tasks regarding singleton types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/follow-up-type-narrowing
created_at: 2024-10-16T08:54:43Z
updated_at: 2024-10-16T09:30:05Z
url: https://github.com/astral-sh/ruff/pull/13769
synced_at: 2026-01-12T15:55:45Z
```

# [red knot] Minor follow-up tasks regarding singleton types

---

_@sharkdp_

## Summary

- Do not treat empty tuples as singletons after discussion [1]
- Improve comment regarding intersection types
- Resolve unnecessary TODO in Markdown test

[1] https://discuss.python.org/t/should-we-specify-in-the-language-reference-that-the-empty-tuple-is-a-singleton/67957

## Test Plan

—

---

_Label `red-knot` added by @sharkdp on 2024-10-16 08:54_

---

_Review requested from @carljm by @sharkdp on 2024-10-16 08:54_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-16 08:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-16 08:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:489 on 2024-10-16 08:58_

maybe something like this?

```suggestion
                // The empty tuple is a singleton on CPython and PyPy,
                // but not on other Python implementations such as GraalPy.
                // Its *use* as a singleton is discouraged and should not be
                // relied on for type narrowing, so we do not treat it as one. See:
```

---

_@AlexWaygood approved on 2024-10-16 09:01_

LGTM. One optional nit to make a comment slightly more precise. Clippy is also now complaining about the `db` parameter being unused in `Type::is_singleton`

---

_Comment by @github-actions[bot] on 2024-10-16 09:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-10-16 09:30_

---

_Closed by @sharkdp on 2024-10-16 09:30_

---

_Branch deleted on 2024-10-16 09:30_

---
