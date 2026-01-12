```yaml
number: 15744
title: "[`ruff`] Do not emit diagnostic when all arguments to `zip()` are variadic (`RUF058`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF058
created_at: 2025-01-25T18:32:41Z
updated_at: 2025-01-25T18:46:04Z
url: https://github.com/astral-sh/ruff/pull/15744
synced_at: 2026-01-12T15:55:52Z
```

# [`ruff`] Do not emit diagnostic when all arguments to `zip()` are variadic (`RUF058`)

---

_@InSyncWithFoo_

## Summary

Resolves #15742.

Previously, these would be considered violations:

```python
# All arguments are variadic
starmap(print, zip(*a))
starmap(print, zip(*a, *b))
```

It is possible, however, that the user is relying on the fact that `zip()` can be called without arguments. On the other hand, `map()` can't be called with less than two arguments.

Thus, after this change, the aforementioned cases are no longer reported.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_@AlexWaygood approved on 2025-01-25 18:39_

Yeah, good call -- they may be using the more verbose idiom precisely _because_ it allows for variadic arguments where `map()` doesn't. So I think you're right; it doesn't make sense to emit the diagnostic here at all.

Thanks!

---

_Label `bug` added by @AlexWaygood on 2025-01-25 18:39_

---

_Label `rule` added by @AlexWaygood on 2025-01-25 18:39_

---

_Comment by @github-actions[bot] on 2025-01-25 18:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `preview` added by @AlexWaygood on 2025-01-25 18:42_

---

_Merged by @AlexWaygood on 2025-01-25 18:42_

---

_Closed by @AlexWaygood on 2025-01-25 18:42_

---

_Branch deleted on 2025-01-25 18:46_

---
