```yaml
number: 15880
title: "[`flake8-pyi`] Avoid an unnecessary `.unwrap()` call in `PYI019` autofix"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/pyi019-unwrap
created_at: 2025-02-02T18:57:51Z
updated_at: 2025-02-02T19:05:53Z
url: https://github.com/astral-sh/ruff/pull/15880
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-pyi`] Avoid an unnecessary `.unwrap()` call in `PYI019` autofix

---

_@AlexWaygood_

## Summary

There's no functional change here. This just gets rid of an unnecessary `.unwrap()` call with a pattern that can be verified by the compiler to be safe.

I also got rid of the `replace_return_annotation_with_self` function, as it is unnecessary if we just extend `replace_references_range` to cover the return annotation as well as the function parameters.

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `internal` added by @AlexWaygood on 2025-02-02 18:57_

---

_Merged by @AlexWaygood on 2025-02-02 19:04_

---

_Closed by @AlexWaygood on 2025-02-02 19:04_

---

_Branch deleted on 2025-02-02 19:04_

---

_Comment by @github-actions[bot] on 2025-02-02 19:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
