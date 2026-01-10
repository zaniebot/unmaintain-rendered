```yaml
number: 11596
title: "Reduce extensive use of `snapshot.query`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: reduce-extensive-use-of-snapshot-query
created_at: 2024-05-29T07:13:42Z
updated_at: 2024-05-29T08:11:47Z
url: https://github.com/astral-sh/ruff/pull/11596
synced_at: 2026-01-10T21:56:00Z
```

# Reduce extensive use of `snapshot.query`

---

_Pull request opened by @MichaReiser on 2024-05-29 07:13_

## Summary

Reduce the extensive use of `snapshot.query` and instead assign the result to a variable and reuse it. 

Also avoid passing unnecessary data to functins that can be retrieved from `query`.

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-05-29 07:15_

---

_@dhruvmanila approved on 2024-05-29 07:24_

---

_Comment by @github-actions[bot] on 2024-05-29 07:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-05-29 08:11_

---

_Closed by @MichaReiser on 2024-05-29 08:11_

---

_Branch deleted on 2024-05-29 08:11_

---
