```yaml
number: 13162
title: "[`flake8-pyi`] Respect `pep8_naming.classmethod-decorators` settings when determining if a method is a classmethod in `custom-type-var-return-type` (`PYI019`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: alex/dunder-new
created_at: 2024-08-30T13:07:03Z
updated_at: 2024-08-30T13:24:03Z
url: https://github.com/astral-sh/ruff/pull/13162
synced_at: 2026-01-12T15:55:43Z
```

# [`flake8-pyi`] Respect `pep8_naming.classmethod-decorators` settings when determining if a method is a classmethod in `custom-type-var-return-type` (`PYI019`)

---

_@AlexWaygood_

## Summary

I noticed that `PYI019` was using its own routines for determining whether a function was a classmethod or not, when most of our linter rules use `ruff_python_semantic::function_type::classify()` for this purpose. Using the common method in `PYI019` means that this rule will benefit from any bugfixes we make to `classify()` in the future, and also means that this rule now respects the `pep8_naming.classmethod-decorators` setting when deciding whether a method is a classmethod or not.

## Test Plan

`cargo test -p ruff_linter`


---

_Label `bug` added by @AlexWaygood on 2024-08-30 13:07_

---

_Label `rule` added by @AlexWaygood on 2024-08-30 13:07_

---

_@charliermarsh approved on 2024-08-30 13:12_

---

_Comment by @github-actions[bot] on 2024-08-30 13:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-08-30 13:24_

---

_Closed by @AlexWaygood on 2024-08-30 13:24_

---

_Branch deleted on 2024-08-30 13:24_

---
