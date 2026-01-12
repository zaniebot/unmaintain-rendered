```yaml
number: 8987
title: "Move `ParenthesizedExpr` to `ruff_python_parser`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: move-parenthesized-expression
created_at: 2023-12-04T05:14:30Z
updated_at: 2023-12-04T05:38:25Z
url: https://github.com/astral-sh/ruff/pull/8987
synced_at: 2026-01-10T23:40:55Z
```

# Move `ParenthesizedExpr` to `ruff_python_parser`

---

_Pull request opened by @MichaReiser on 2023-12-04 05:14_

## Summary

`ParenthesizedExpr` is some parser internal hack to make `WithItem` parsing possible. `ParenthesizedExpr` isn't part of our public AST. 

That's why this PR moves `ParenthesizedExpr` into the `ruff_python_parser` crate with `pub(super)` visibility. 

## Test Plan

`cargo test`


---

_Comment by @MichaReiser on 2023-12-04 05:14_

Current dependencies on/for this PR:
* main
  * **PR #8987** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8987?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8987?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-12-04 05:15_

---

_@MichaReiser reviewed on 2023-12-04 05:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser.rs`:439 on 2023-12-04 05:16_

I changed the condition here to only test the start as this should be sufficient

---

_@charliermarsh approved on 2023-12-04 05:16_

---

_Merged by @MichaReiser on 2023-12-04 05:36_

---

_Closed by @MichaReiser on 2023-12-04 05:36_

---

_Branch deleted on 2023-12-04 05:36_

---

_Comment by @github-actions[bot] on 2023-12-04 05:38_

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
