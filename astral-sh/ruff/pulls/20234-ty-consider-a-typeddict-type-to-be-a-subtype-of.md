```yaml
number: 20234
title: "[ty] Consider a TypedDict type to be a subtype of itself"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/typeddict-property-tests
created_at: 2025-09-04T13:57:50Z
updated_at: 2025-09-04T14:03:53Z
url: https://github.com/astral-sh/ruff/pull/20234
synced_at: 2026-01-12T15:56:57Z
```

# [ty] Consider a TypedDict type to be a subtype of itself

---

_@sharkdp_

## Summary

I only wanted to add some `TypedDict` instances to the property tests for later when we implement proper structural assignability and subtyping, but this immediately uncovered a bug where a `TypedDict` type was not considered to be a subtype of itself. Unions and intersections were also not handled correctly, because the `has_relation_to` branch was too high up.

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-09-04 13:57_

---

_Comment by @github-actions[bot] on 2025-09-04 14:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-04 14:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
