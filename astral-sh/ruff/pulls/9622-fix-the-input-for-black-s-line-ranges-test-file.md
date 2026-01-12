```yaml
number: 9622
title: "Fix the input for black's line ranges test file"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: fix-line-range-input-files
created_at: 2024-01-23T10:34:05Z
updated_at: 2024-01-23T10:44:20Z
url: https://github.com/astral-sh/ruff/pull/9622
synced_at: 2026-01-12T15:55:29Z
```

# Fix the input for black's line ranges test file

---

_@MichaReiser_

## Summary

Black's snapshot testing uses a `# flags: --option=value` comment at the top of snapshot files to configure CLI arguments. 
Black's testing infrastructure strips that comment before calling black, except when the flags comment contains the `--line-ranges` flag, in which case the comment is preserved. 

This PR updates our import script to preserve the `# flags` comment if it contains the `--line-ranges` flag similar to Black's testing infrastructure. It reimports the 
Black test cases and updates the snapshots. This PR contains no change outside our testing infrastructure.

## Test Plan

cargo test


---

_Label `internal` added by @MichaReiser on 2024-01-23 10:34_

---

_Label `formatter` added by @MichaReiser on 2024-01-23 10:34_

---

_Merged by @MichaReiser on 2024-01-23 10:40_

---

_Closed by @MichaReiser on 2024-01-23 10:40_

---

_Branch deleted on 2024-01-23 10:40_

---

_Comment by @github-actions[bot] on 2024-01-23 10:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
