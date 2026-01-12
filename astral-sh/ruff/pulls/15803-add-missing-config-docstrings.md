```yaml
number: 15803
title: Add missing config docstrings
type: pull_request
state: merged
author: mishamsk
labels:
  - documentation
assignees: []
merged: true
base: main
head: 4551-add-missing-config-docstrings
created_at: 2025-01-29T02:08:21Z
updated_at: 2025-01-29T03:39:56Z
url: https://github.com/astral-sh/ruff/pull/15803
synced_at: 2026-01-12T15:55:52Z
```

# Add missing config docstrings

---

_@mishamsk_

## Summary

As promised in #15603 - the **highly** sophisticated change - adding missing config docstrings that are used in command completions.

## Test Plan

I actually made a local change to emit all empty items and verified there are none now, before opening the PR.


---

_Comment by @mishamsk on 2025-01-29 02:08_

@dhruvmanila as promised!

---

_Comment by @github-actions[bot] on 2025-01-29 02:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2025-01-29 03:31_

Thank you!

This will also be used in the documentation similar to how there's a "Configures Ruff's `analyze` command" in https://docs.astral.sh/ruff/settings/#analyze.

---

_Label `documentation` added by @dhruvmanila on 2025-01-29 03:31_

---

_Merged by @dhruvmanila on 2025-01-29 03:32_

---

_Closed by @dhruvmanila on 2025-01-29 03:32_

---

_Branch deleted on 2025-01-29 03:39_

---
