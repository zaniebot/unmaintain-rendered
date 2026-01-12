```yaml
number: 11241
title: Make libc a platform specific dependency
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-libc-dependency
created_at: 2024-05-02T07:27:55Z
updated_at: 2024-05-02T07:50:12Z
url: https://github.com/astral-sh/ruff/pull/11241
synced_at: 2026-01-12T15:55:37Z
```

# Make libc a platform specific dependency

---

_@MichaReiser_

## Summary

The `libc` dependency in `ruff_server` is only used on apple targets. Make it a platform specific dependency.

## Test Plan

`cargo test`


---

_Review requested from @snowsignal by @MichaReiser on 2024-05-02 07:28_

---

_Label `internal` added by @MichaReiser on 2024-05-02 07:28_

---

_Renamed from "Remove the libc dependency from ruff-server" to "Make libc a platform specific dependency" by @MichaReiser on 2024-05-02 07:36_

---

_Merged by @MichaReiser on 2024-05-02 07:45_

---

_Closed by @MichaReiser on 2024-05-02 07:45_

---

_Branch deleted on 2024-05-02 07:45_

---

_Comment by @github-actions[bot] on 2024-05-02 07:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
