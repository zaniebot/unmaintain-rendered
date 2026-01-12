```yaml
number: 10064
title: Fix ecosystem
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: ecosystem-check
created_at: 2024-02-20T14:50:55Z
updated_at: 2024-02-20T15:54:51Z
url: https://github.com/astral-sh/ruff/pull/10064
synced_at: 2026-01-12T15:55:31Z
```

# Fix ecosystem

---

_@MichaReiser_


## Summary

I don't understand why our ecosystem check suddenly started failing but the reason is that the `dawidd6/action-download-artifact` action now uses the release workflow instead of the `CI` workflow. I've honestly been too lazy to debug what happened and hardcoding the workflow works as expected.

## Test Plan

The ecosystem checks on this branch are passing


---

_Label `ci` added by @MichaReiser on 2024-02-20 15:12_

---

_Comment by @github-actions[bot] on 2024-02-20 15:34_

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

_@zanieb approved on 2024-02-20 15:39_

Yeah seems reasonable

---

_Marked ready for review by @MichaReiser on 2024-02-20 15:54_

---

_Merged by @MichaReiser on 2024-02-20 15:54_

---

_Closed by @MichaReiser on 2024-02-20 15:54_

---

_Branch deleted on 2024-02-20 15:54_

---
