```yaml
number: 20741
title: Move --show-settings snapshots to separate files
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: show-settings
created_at: 2025-10-07T09:33:59Z
updated_at: 2025-10-07T09:43:30Z
url: https://github.com/astral-sh/ruff/pull/20741
synced_at: 2026-01-12T15:57:08Z
```

# Move --show-settings snapshots to separate files

---

_@MichaReiser_

## Summary

This is a follow up to https://github.com/astral-sh/ruff/pull/20689

The `--show-settings` snapshot account for about 40% of the code in `lint.rs`. 
This PR moves those snapshots to file-snapshots to reduce the overall size of the file, which,
IMO improves overall readability (and my editor stops complaining that the file is over 5000 lines long).

## Test Plan

cargo test


---

_Label `testing` added by @MichaReiser on 2025-10-07 09:34_

---

_Merged by @MichaReiser on 2025-10-07 09:42_

---

_Closed by @MichaReiser on 2025-10-07 09:42_

---

_Branch deleted on 2025-10-07 09:42_

---

_Comment by @github-actions[bot] on 2025-10-07 09:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
