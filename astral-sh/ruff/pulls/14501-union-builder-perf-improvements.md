```yaml
number: 14501
title: Union builder perf improvements
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/union-add-first
created_at: 2024-11-20T20:45:08Z
updated_at: 2024-11-20T20:55:32Z
url: https://github.com/astral-sh/ruff/pull/14501
synced_at: 2026-01-12T15:55:48Z
```

# Union builder perf improvements

---

_@MichaReiser_

## Summary

* Use `SmallVec` for `UnionBuilder` to avoid allocationg when creating a union from a single type
* Short-circuit in `add` when the union is empty

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-11-20 20:45_

---

_Comment by @MichaReiser on 2024-11-20 20:53_

Hmm, less than 1% change. 

---

_Closed by @MichaReiser on 2024-11-20 20:53_

---

_Comment by @github-actions[bot] on 2024-11-20 20:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
