```yaml
number: 13308
title: "[`isort`] Improve rule documentation with a link to the option (`I002`)"
type: pull_request
state: merged
author: dizzy57
labels:
  - documentation
assignees: []
merged: true
base: main
head: isort-add-options-link
created_at: 2024-09-10T12:58:07Z
updated_at: 2024-09-13T14:07:09Z
url: https://github.com/astral-sh/ruff/pull/13308
synced_at: 2026-01-12T15:55:43Z
```

# [`isort`] Improve rule documentation with a link to the option (`I002`)

---

_@dizzy57_

## Summary

https://docs.astral.sh/ruff/rules/missing-required-import/ is missing a link to https://docs.astral.sh/ruff/settings/#lint_isort_required-imports that controls this rule's behavior.

## Test Plan

`cargo dev generate-docs`
`grep 'lint.isort.required-imports' docs/rules/missing-required-import.md`

---

_Comment by @github-actions[bot] on 2024-09-10 13:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-09-10 13:36_

Thanks!

---

_Merged by @AlexWaygood on 2024-09-10 13:36_

---

_Closed by @AlexWaygood on 2024-09-10 13:36_

---

_Branch deleted on 2024-09-10 13:41_

---

_Label `documentation` added by @dhruvmanila on 2024-09-13 14:07_

---
