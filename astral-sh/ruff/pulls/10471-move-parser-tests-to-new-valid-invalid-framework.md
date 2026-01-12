```yaml
number: 10471
title: Move parser tests to new valid/invalid framework
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/move-tests
created_at: 2024-03-19T12:17:01Z
updated_at: 2024-04-17T15:13:47Z
url: https://github.com/astral-sh/ruff/pull/10471
synced_at: 2026-01-12T15:55:32Z
```

# Move parser tests to new valid/invalid framework

---

_@dhruvmanila_

## Summary

This PR is just a mechanical change to move the existing parser tests to use the new valid/invalid framework. It also removes duplicate test cases and rearranges wherever feasible.

The with item and IPython syntax tests aren't ported yet. The former will be done once the #10418 PR is merged while the latter will be done later.

The snapshots were updated and old snapshots were removed.

## Test Plan

`cargo test`


---

_Label `parser` added by @dhruvmanila on 2024-03-19 12:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-19 12:17_

---

_Comment by @github-actions[bot] on 2024-03-19 12:37_

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

_@MichaReiser reviewed on 2024-03-19 12:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/fixtures.rs`:31 on 2024-03-19 12:45_

whoops

---

_@MichaReiser approved on 2024-03-19 12:45_

Looks good. I didn't review the Python files or create a snapshot, but I assume they must all be good, considering that this PR contains no logic changes.

---

_Merged by @dhruvmanila on 2024-03-19 12:46_

---

_Closed by @dhruvmanila on 2024-03-19 12:46_

---

_Branch deleted on 2024-03-19 12:46_

---
