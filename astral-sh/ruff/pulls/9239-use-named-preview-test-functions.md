```yaml
number: 9239
title: Use named preview test functions
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: use-preview-style-specific-test-functions
created_at: 2023-12-22T00:14:24Z
updated_at: 2023-12-22T00:27:00Z
url: https://github.com/astral-sh/ruff/pull/9239
synced_at: 2026-01-12T15:55:28Z
```

# Use named preview test functions

---

_@MichaReiser_

## Summary

Use preview style specific `is_..._enabled` instead of the generic `options().preview().is_enabled()` to better document
which preview style a preview test is gating.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2023-12-22 00:17_

---

_Merged by @MichaReiser on 2023-12-22 00:23_

---

_Closed by @MichaReiser on 2023-12-22 00:23_

---

_Branch deleted on 2023-12-22 00:23_

---

_Comment by @github-actions[bot] on 2023-12-22 00:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
