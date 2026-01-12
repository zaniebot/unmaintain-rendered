```yaml
number: 14643
title: "Fixes minor bug in `SemanticModel::lookup_symbol`"
type: pull_request
state: merged
author: Daverball
labels: []
assignees: []
merged: true
base: main
head: bugfix/lookup_symbol
created_at: 2024-11-27T21:36:54Z
updated_at: 2024-11-28T15:13:04Z
url: https://github.com/astral-sh/ruff/pull/14643
synced_at: 2026-01-12T15:55:48Z
```

# Fixes minor bug in `SemanticModel::lookup_symbol`

---

_@Daverball_

## Summary

This came up as part of #12927 when implementing `SemanticModel::simulate_runtime_load`.

Should be fairly self-explanatory, if the scope returns a binding with `BindingKind::Annotation` the bottom part of the loop gets skipped, so there's no chance for `seen_function` to have been updated. So unless there's something subtle going on here, like function scopes never containing bindings with `BindingKind::Annotation`, this seems like a bug.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-11-27 21:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-11-27 21:50_

---

_Merged by @charliermarsh on 2024-11-27 21:50_

---

_Closed by @charliermarsh on 2024-11-27 21:50_

---

_Comment by @charliermarsh on 2024-11-27 21:50_

Thanks for following up! I really appreciate it.

---

_Branch deleted on 2024-11-28 15:13_

---
