```yaml
number: 15609
title: "Avoid large closure to make `rustfmt` work"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/refactor-bandit-match
created_at: 2025-01-20T09:11:00Z
updated_at: 2025-01-20T09:21:22Z
url: https://github.com/astral-sh/ruff/pull/15609
synced_at: 2026-01-12T15:55:51Z
```

# Avoid large closure to make `rustfmt` work

---

_@dhruvmanila_

## Summary

I noticed this while reviewing https://github.com/astral-sh/ruff/pull/15541 that the code inside the large closure cannot be formatted by the Rust formatter. This PR extracts the qualified name and inlines the match expression.

## Test Plan

`cargo clippy` and `cargo insta`

---

_Label `internal` added by @dhruvmanila on 2025-01-20 09:11_

---

_Merged by @dhruvmanila on 2025-01-20 09:15_

---

_Closed by @dhruvmanila on 2025-01-20 09:15_

---

_Branch deleted on 2025-01-20 09:15_

---

_Comment by @github-actions[bot] on 2025-01-20 09:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
