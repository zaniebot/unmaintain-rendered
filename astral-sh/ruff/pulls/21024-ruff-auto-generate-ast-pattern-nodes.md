```yaml
number: 21024
title: "[`ruff`] Auto generate ast Pattern nodes"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: autogenerate-pattern-nodes
created_at: 2025-10-21T20:09:54Z
updated_at: 2025-10-22T06:49:23Z
url: https://github.com/astral-sh/ruff/pull/21024
synced_at: 2026-01-12T15:57:14Z
```

# [`ruff`] Auto generate ast Pattern nodes

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a part of #15655

Auto generate ast Pattern nodes

## Test Plan

<!-- How was it tested? -->

I have fixed some syntax tests. All other tests should pass without any changes. I have confirmed the generated code to make sure it's the same as old code.


---

_Renamed from "Auto generate ast Pattern nodes" to "[`ruff`] Auto generate ast Pattern nodes" by @TaKO8Ki on 2025-10-21 20:10_

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-10-21 20:26_

---

_Review requested from @dhruvmanila by @TaKO8Ki on 2025-10-21 20:26_

---

_Comment by @TaKO8Ki on 2025-10-21 20:32_

I have fixed some snapshots in `ruff_python_parser/tests`. If we change the order of node_index and range in `generate.py`, It's unnecessary. However, other definitions such as `StmtExpr` is already based on the fields order, so I will keep the following order.

```rust
pub node_index: crate::AtomicNodeIndex,
pub range: ruff_text_size::TextRange,
```

---

_Comment by @github-actions[bot] on 2025-10-21 21:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @ntBre by @amyreese on 2025-10-22 02:02_

---

_Label `parser` added by @amyreese on 2025-10-22 02:04_

---

_@MichaReiser approved on 2025-10-22 06:24_

Thank you

---

_Label `internal` added by @MichaReiser on 2025-10-22 06:24_

---

_Merged by @MichaReiser on 2025-10-22 06:24_

---

_Closed by @MichaReiser on 2025-10-22 06:24_

---

_Branch deleted on 2025-10-22 06:49_

---
