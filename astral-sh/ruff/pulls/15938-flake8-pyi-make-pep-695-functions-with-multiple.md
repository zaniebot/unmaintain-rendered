```yaml
number: 15938
title: "[`flake8-pyi`] Make PEP-695 functions with multiple type parameters fixable by PYI019 again"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: alex/pyi019-bug
created_at: 2025-02-04T14:31:23Z
updated_at: 2025-02-04T14:39:49Z
url: https://github.com/astral-sh/ruff/pull/15938
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-pyi`] Make PEP-695 functions with multiple type parameters fixable by PYI019 again

---

_@AlexWaygood_

## Summary

I realised immediately after landing #15935 that the change introduced a small bug in PYI019, which meant that functions with multiple type parameters would no longer be autofixed if one of the type parameters had a bound or constraint. The bug slipped through in #15935 because we didn't have any test coverage for this specific edge case (which shows the risk of that kind of change!).

This PR fixes the new bug and adds test coverage for the edge case. I'm adding the "internal" label: this shouldn't be included in the changelog, because it's a new bug that hasn't been included in any release of Ruff.

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `bug` added by @AlexWaygood on 2025-02-04 14:31_

---

_Label `internal` added by @AlexWaygood on 2025-02-04 14:31_

---

_Merged by @AlexWaygood on 2025-02-04 14:38_

---

_Closed by @AlexWaygood on 2025-02-04 14:38_

---

_Branch deleted on 2025-02-04 14:38_

---

_Comment by @github-actions[bot] on 2025-02-04 14:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
