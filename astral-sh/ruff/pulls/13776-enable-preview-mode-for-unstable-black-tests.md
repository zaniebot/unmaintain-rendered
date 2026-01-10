```yaml
number: 13776
title: "Enable preview mode for 'unstable' black tests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: micha/black-tests-unstable-style
created_at: 2024-10-16T12:20:36Z
updated_at: 2024-10-16T12:31:44Z
url: https://github.com/astral-sh/ruff/pull/13776
synced_at: 2026-01-10T20:59:37Z
```

# Enable preview mode for 'unstable' black tests

---

_Pull request opened by @MichaReiser on 2024-10-16 12:20_

## Summary

This PR updates the Black test corpus and enables preview mode for all tests that require `unstable` in black (similar to preview but these styles have known bugs)

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-10-16 12:20_

---

_Label `formatter` added by @MichaReiser on 2024-10-16 12:20_

---

_Merged by @MichaReiser on 2024-10-16 12:25_

---

_Closed by @MichaReiser on 2024-10-16 12:25_

---

_Branch deleted on 2024-10-16 12:25_

---

_Comment by @github-actions[bot] on 2024-10-16 12:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
