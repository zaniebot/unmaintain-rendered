```yaml
number: 12907
title: "Simplify logic for `RUF027`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/ruf027-2
created_at: 2024-08-15T16:24:51Z
updated_at: 2024-08-16T07:05:17Z
url: https://github.com/astral-sh/ruff/pull/12907
synced_at: 2026-01-12T15:55:42Z
```

# Simplify logic for `RUF027`

---

_@AlexWaygood_

## Summary

This PR is a pure refactor to simplify some of the logic for `RUF027`. This will make it easier to file some followup PRs to help reduce the false positives from this rule. I'm separating the refactor out into a separate PR so it's easier to review, and so I can double-check from the ecosystem report that this doesn't have any user-facing impact.

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `internal` added by @AlexWaygood on 2024-08-15 16:27_

---

_Comment by @github-actions[bot] on 2024-08-15 16:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @AlexWaygood on 2024-08-15 17:45_

---

_@charliermarsh approved on 2024-08-16 00:19_

---

_Merged by @AlexWaygood on 2024-08-16 07:05_

---

_Closed by @AlexWaygood on 2024-08-16 07:05_

---

_Branch deleted on 2024-08-16 07:05_

---
