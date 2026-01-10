```yaml
number: 11240
title: Remove num-cpus dependency
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-num-cpus
created_at: 2024-05-02T07:15:15Z
updated_at: 2024-05-02T20:30:49Z
url: https://github.com/astral-sh/ruff/pull/11240
synced_at: 2026-01-10T22:37:02Z
```

# Remove num-cpus dependency

---

_Pull request opened by @MichaReiser on 2024-05-02 07:15_

## Summary

Use the `std::thread::available_parallelism` function over `num_cpus` to reduce our dependencies.

## Test Plan

`cargo test`


---

_Review requested from @snowsignal by @MichaReiser on 2024-05-02 07:15_

---

_Label `internal` added by @MichaReiser on 2024-05-02 07:15_

---

_Comment by @github-actions[bot] on 2024-05-02 07:39_

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

_@snowsignal approved on 2024-05-02 16:52_

---

_@zanieb approved on 2024-05-02 17:36_

---

_Merged by @MichaReiser on 2024-05-02 20:30_

---

_Closed by @MichaReiser on 2024-05-02 20:30_

---

_Branch deleted on 2024-05-02 20:30_

---
