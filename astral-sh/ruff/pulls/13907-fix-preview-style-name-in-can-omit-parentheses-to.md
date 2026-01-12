```yaml
number: 13907
title: "Fix preview style name in `can_omit_parentheses` to `is_f_string_formatting_enabled`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/fix-can-omit-preview-style
created_at: 2024-10-24T11:27:30Z
updated_at: 2024-10-24T11:38:45Z
url: https://github.com/astral-sh/ruff/pull/13907
synced_at: 2026-01-12T15:55:46Z
```

# Fix preview style name in `can_omit_parentheses` to `is_f_string_formatting_enabled`

---

_@MichaReiser_


## Summary

I messed up the gating of the preview style in `can_omit_parenhteses`. The logic applies to `is_f_string_formatting_enabled`.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-10-24 11:27_

---

_Renamed from "Fix preview style name in `can_omit_parentheses` to is_f_string_formatting_enabled" to "Fix preview style name in `can_omit_parentheses` to `is_f_string_formatting_enabled`" by @MichaReiser on 2024-10-24 11:30_

---

_Merged by @MichaReiser on 2024-10-24 11:32_

---

_Closed by @MichaReiser on 2024-10-24 11:32_

---

_Branch deleted on 2024-10-24 11:32_

---

_Comment by @github-actions[bot] on 2024-10-24 11:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
