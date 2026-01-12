```yaml
number: 18892
title: "fix casing of `analyze.direction` variant names"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: micha/fix-analyze-direction-casing
created_at: 2025-06-23T11:02:40Z
updated_at: 2025-06-23T13:22:09Z
url: https://github.com/astral-sh/ruff/pull/18892
synced_at: 2026-01-12T15:56:27Z
```

# fix casing of `analyze.direction` variant names

---

_@MichaReiser_

## Summary

Fixes `analyze.direction` to use kebab-case for the variant names. 

Fixes https://github.com/astral-sh/ruff/issues/18887

## Test Plan

Created a `ruff.toml` and tested that both `dependents` and `Dependents` were accepted


---

_Label `bug` added by @MichaReiser on 2025-06-23 11:02_

---

_Label `configuration` added by @MichaReiser on 2025-06-23 11:02_

---

_Comment by @github-actions[bot] on 2025-06-23 11:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-06-23 12:08_

---

_Merged by @MichaReiser on 2025-06-23 12:30_

---

_Closed by @MichaReiser on 2025-06-23 12:30_

---

_Branch deleted on 2025-06-23 12:30_

---

_Comment by @charliermarsh on 2025-06-23 13:22_

Thanks!

---
