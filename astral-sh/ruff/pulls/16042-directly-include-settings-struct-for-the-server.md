```yaml
number: 16042
title: "Directly include `Settings` struct for the server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/ruff-settings
created_at: 2025-02-08T17:55:00Z
updated_at: 2025-02-10T04:50:03Z
url: https://github.com/astral-sh/ruff/pull/16042
synced_at: 2026-01-10T19:57:22Z
```

# Directly include `Settings` struct for the server

---

_Pull request opened by @dhruvmanila on 2025-02-08 17:55_

## Summary

This PR refactors the `RuffSettings` struct to directly include the resolved `Settings` instead of including the specific fields from it. The server utilizes a lot of it already, so it makes sense to just include the entire struct for simplicity.

### `Deref`

I implemented `Deref` on `RuffSettings` to return the `Settings` because `RuffSettings` is now basically a wrapper around it with the config path as the other field. This path field is only used for debugging ("printDebugInformation" command).


---

_Label `internal` added by @dhruvmanila on 2025-02-08 17:55_

---

_@dhruvmanila reviewed on 2025-02-08 17:57_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/fix.rs`:39 on 2025-02-08 17:57_

I'm still passing these individually to `is_document_excluded_*` functions because some paths require a different linter settings (e.g., organize imports uses a specific settings which only selects `I`).

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-08 18:00_

---

_Comment by @github-actions[bot] on 2025-02-08 18:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-02-09 14:01_

---

_Merged by @dhruvmanila on 2025-02-10 04:50_

---

_Closed by @dhruvmanila on 2025-02-10 04:50_

---

_Branch deleted on 2025-02-10 04:50_

---
