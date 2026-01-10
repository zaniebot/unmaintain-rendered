```yaml
number: 11129
title: Use crossbeam-channel instead of crossbeam
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: use-crossbeam-channel
created_at: 2024-04-24T13:48:42Z
updated_at: 2024-04-24T14:06:48Z
url: https://github.com/astral-sh/ruff/pull/11129
synced_at: 2026-01-10T22:37:01Z
```

# Use crossbeam-channel instead of crossbeam

---

_Pull request opened by @MichaReiser on 2024-04-24 13:48_

## Summary

We only use crossbeam channels, but none of the other crossbeam functionalities. 

That's why this PR replaces the `crossbeam` dependency with `crossbeam-channel` to reduce the number of our dependencies.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-04-24 13:48_

---

_@charliermarsh approved on 2024-04-24 13:49_

Nice

---

_Merged by @MichaReiser on 2024-04-24 13:56_

---

_Closed by @MichaReiser on 2024-04-24 13:56_

---

_Branch deleted on 2024-04-24 13:56_

---

_Comment by @github-actions[bot] on 2024-04-24 14:06_

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
