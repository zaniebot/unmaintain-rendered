```yaml
number: 14929
title: Add tests demonstrating f-strings with debug expressions in replacements that contain escaped characters
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/fstring-debug-expression-tests
created_at: 2024-12-12T09:28:43Z
updated_at: 2024-12-12T09:35:08Z
url: https://github.com/astral-sh/ruff/pull/14929
synced_at: 2026-01-12T15:55:49Z
```

# Add tests demonstrating f-strings with debug expressions in replacements that contain escaped characters

---

_@MichaReiser_

## Summary

This PR adds two test cases that were reported in https://github.com/astral-sh/ruff/issues/14766 and https://github.com/astral-sh/ruff/issues/14926

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-12-12 09:28_

---

_@MichaReiser reviewed on 2024-12-12 09:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:181 on 2024-12-12 09:29_

Ha, that took me a minute to figure out. We always formatted the code once with the default options even if there's an options file. We don't want that, so I moved it into the `else` branch`

---

_Merged by @MichaReiser on 2024-12-12 09:33_

---

_Closed by @MichaReiser on 2024-12-12 09:33_

---

_Branch deleted on 2024-12-12 09:33_

---

_Comment by @github-actions[bot] on 2024-12-12 09:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
