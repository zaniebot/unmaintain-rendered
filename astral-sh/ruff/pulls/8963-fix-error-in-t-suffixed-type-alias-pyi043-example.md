```yaml
number: 8963
title: "Fix error in `t-suffixed-type-alias` (`PYI043`) example"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: pyi043-fix-docs
created_at: 2023-12-02T13:00:45Z
updated_at: 2023-12-02T14:33:08Z
url: https://github.com/astral-sh/ruff/pull/8963
synced_at: 2026-01-12T15:55:27Z
```

# Fix error in `t-suffixed-type-alias` (`PYI043`) example

---

_@tjkuson_

## Summary

For `t-suffixed-type-alias` to trigger, the type alias needs to be marked as such using the `typing.TypeAlias` annotation and the name of the alias must be marked as private using a leading underscore. The documentation example was of an unannotated type alias that was not marked as private, which was misleading.

## Test Plan

The current example doesn't trigger the rule; the example in this merge request does.


---

_Renamed from "Fix error`t-suffixed-type-alias` (`PYI043`) example" to "Fix error in `t-suffixed-type-alias` (`PYI043`) example" by @tjkuson on 2023-12-02 13:01_

---

_Marked ready for review by @tjkuson on 2023-12-02 13:03_

---

_Comment by @github-actions[bot] on 2023-12-02 13:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-12-02 13:52_

---

_Label `documentation` added by @charliermarsh on 2023-12-02 13:52_

---

_Merged by @charliermarsh on 2023-12-02 13:52_

---

_Closed by @charliermarsh on 2023-12-02 13:52_

---

_Branch deleted on 2023-12-02 14:33_

---
