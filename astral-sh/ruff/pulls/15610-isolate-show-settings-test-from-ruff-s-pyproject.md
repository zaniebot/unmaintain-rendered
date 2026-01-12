```yaml
number: 15610
title: "Isolate `show_settings` test from Ruff's `pyproject.toml"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/isolate-show-settings-test
created_at: 2025-01-20T09:18:20Z
updated_at: 2025-01-20T09:31:21Z
url: https://github.com/astral-sh/ruff/pull/15610
synced_at: 2026-01-12T15:55:51Z
```

# Isolate `show_settings` test from Ruff's `pyproject.toml

---

_@MichaReiser_

## Summary

In preperation for https://github.com/astral-sh/ruff/pull/15558

Isolate the `show_settings` test instead of reading Ruff's `pyproject.toml` for better test isolation.

## Test Plan

`cargo test`


---

_Label `testing` added by @MichaReiser on 2025-01-20 09:20_

---

_Merged by @MichaReiser on 2025-01-20 09:28_

---

_Closed by @MichaReiser on 2025-01-20 09:28_

---

_Branch deleted on 2025-01-20 09:28_

---

_Comment by @github-actions[bot] on 2025-01-20 09:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-01-20 09:31_

Looks like the Windows CI failed here: https://github.com/astral-sh/ruff/actions/runs/12864831691/job/35864149743

---
