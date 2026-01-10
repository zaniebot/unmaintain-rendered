```yaml
number: 11201
title: "linter: Enable test-rules for test build"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: test-rules-linter
created_at: 2024-04-29T12:27:11Z
updated_at: 2024-04-30T06:06:48Z
url: https://github.com/astral-sh/ruff/pull/11201
synced_at: 2026-01-10T22:37:02Z
```

# linter: Enable test-rules for test build

---

_Pull request opened by @MichaReiser on 2024-04-29 12:27_

## Summary

`ruff_linter__rules__ruff__tests__RUF101_RUF101` started failing because it relies on the `test-rules` feature to be enabled in `ruff_linter`. 

This PR enables the `test-rules` when building for tests or when the feature is enabled. 

## Test Plan

`cargo test -p ruff_linter` passes


---

_Label `internal` added by @MichaReiser on 2024-04-29 12:27_

---

_Review requested from @zanieb by @MichaReiser on 2024-04-29 12:27_

---

_Comment by @github-actions[bot] on 2024-04-29 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-04-29 16:56_

Makes sense to me, and fixes things for me locally!

---

_Merged by @MichaReiser on 2024-04-30 06:06_

---

_Closed by @MichaReiser on 2024-04-30 06:06_

---

_Branch deleted on 2024-04-30 06:06_

---
