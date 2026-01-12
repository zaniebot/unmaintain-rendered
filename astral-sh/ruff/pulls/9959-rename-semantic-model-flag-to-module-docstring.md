```yaml
number: 9959
title: "Rename semantic model flag to `MODULE_DOCSTRING_BOUNDARY`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/rename-model-flag
created_at: 2024-02-12T19:05:55Z
updated_at: 2024-02-12T19:18:41Z
url: https://github.com/astral-sh/ruff/pull/9959
synced_at: 2026-01-12T15:55:30Z
```

# Rename semantic model flag to `MODULE_DOCSTRING_BOUNDARY`

---

_@dhruvmanila_

## Summary

This PR renames the semantic model flag `MODULE_DOCSTRING` to `MODULE_DOCSTRING_BOUNDARY`. The main reason is for readability and for the new semantic model flag `DOCSTRING` which tracks that the model is in a module / class / function docstring.

I got confused earlier with the name until I looked at the use case and it seems that the `_BOUNDARY` prefix is more appropriate for the use-case and is consistent with other flags.

---

_Label `internal` added by @dhruvmanila on 2024-02-12 19:05_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-02-12 19:05_

---

_@charliermarsh approved on 2024-02-12 19:12_

---

_Merged by @dhruvmanila on 2024-02-12 19:17_

---

_Closed by @dhruvmanila on 2024-02-12 19:17_

---

_Branch deleted on 2024-02-12 19:17_

---

_Comment by @github-actions[bot] on 2024-02-12 19:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
