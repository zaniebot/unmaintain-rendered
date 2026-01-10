```yaml
number: 21259
title: "[`refurb`] Expand fix safety for keyword arguments and `Decimal`s (`FURB164`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: furb164
created_at: 2025-11-03T16:18:22Z
updated_at: 2025-11-04T10:11:57Z
url: https://github.com/astral-sh/ruff/pull/21259
synced_at: 2026-01-10T16:53:55Z
```

# [`refurb`] Expand fix safety for keyword arguments and `Decimal`s (`FURB164`)

---

_Pull request opened by @chirizxc on 2025-11-03 16:18_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/21257

## Test Plan

`cargo nextest run furb164`


---

_Comment by @github-actions[bot] on 2025-11-03 16:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @ntBre on 2025-11-03 20:31_

---

_Label `preview` added by @ntBre on 2025-11-03 20:31_

---

_@ntBre approved on 2025-11-03 21:07_

Thank you! It would be kind of nice if we could use our limited [type inference](https://github.com/astral-sh/ruff/blob/f64bbb45b9d7249138d65cc9b9ac6b87f74eef1b/crates/ruff_python_semantic/src/analyze/typing.rs#L1) infrastructure to check `Decimal` types for annotated variables and not just the direct constructor calls, but this seems like a good step in the right direction.

---

_Renamed from "[`refurb`] Fix `FURB164` fixes" to "[`refurb`] Expand fix safety for keyword arguments and `Decimal`s (`FURB164`)" by @ntBre on 2025-11-03 21:08_

---

_Merged by @ntBre on 2025-11-03 21:09_

---

_Closed by @ntBre on 2025-11-03 21:09_

---

_Branch deleted on 2025-11-04 10:11_

---
