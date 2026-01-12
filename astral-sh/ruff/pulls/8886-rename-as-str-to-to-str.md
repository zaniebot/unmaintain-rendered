```yaml
number: 8886
title: "Rename `as_str` to `to_str`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/rename-method
created_at: 2023-11-29T00:40:46Z
updated_at: 2023-11-29T00:56:37Z
url: https://github.com/astral-sh/ruff/pull/8886
synced_at: 2026-01-12T15:55:27Z
```

# Rename `as_str` to `to_str`

---

_@dhruvmanila_

This PR renames the method on `StringLiteralValue` from `as_str` to `to_str`. The main motivation is to follow the naming convention as described in the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv). This method can perform a string allocation in case the string is implicitly concatenated.


---

_Comment by @dhruvmanila on 2023-11-29 00:40_

Current dependencies on/for this PR:
* main
  * **PR #8886** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8886?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8886?utm_source=stack-comment).

---

_Label `internal` added by @dhruvmanila on 2023-11-29 00:50_

---

_Merged by @dhruvmanila on 2023-11-29 00:50_

---

_Closed by @dhruvmanila on 2023-11-29 00:50_

---

_Branch deleted on 2023-11-29 00:50_

---

_Comment by @github-actions[bot] on 2023-11-29 00:56_

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
