```yaml
number: 17703
title: "[red-knot] micro-optimise `ClassLiteral::is_protocol`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/micro-optimise-is-protocol
created_at: 2025-04-29T12:32:26Z
updated_at: 2025-04-29T12:35:55Z
url: https://github.com/astral-sh/ruff/pull/17703
synced_at: 2026-01-12T15:56:04Z
```

# [red-knot] micro-optimise `ClassLiteral::is_protocol`

---

_@AlexWaygood_

## Summary

I doubt it makes any significant difference to performance, but if `Protocol` is present in the bases list of a valid protocol class, it must either:
1. be the last base
2. or be the last-but-one base (with the final base being `Generic[]` or `object`)
3. or be the last-but-two base (with the penultimate base being `Generic[]` and the final base being `object`)

## Test Plan

Existing tests pass


---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 12:32_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-29 12:32_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-29 12:32_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-29 12:32_

---

_Comment by @github-actions[bot] on 2025-04-29 12:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-04-29 12:35_

---

_Closed by @AlexWaygood on 2025-04-29 12:35_

---

_Branch deleted on 2025-04-29 12:35_

---
