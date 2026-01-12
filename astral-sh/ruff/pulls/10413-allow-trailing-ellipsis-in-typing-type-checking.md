```yaml
number: 10413
title: "Allow trailing ellipsis in `typing.TYPE_CHECKING`"
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
assignees: []
merged: true
base: main
head: allow-ellipsis-if-type-checking
created_at: 2024-03-14T21:55:00Z
updated_at: 2024-03-15T08:47:51Z
url: https://github.com/astral-sh/ruff/pull/10413
synced_at: 2026-01-12T15:55:32Z
```

# Allow trailing ellipsis in `typing.TYPE_CHECKING`

---

_@tjkuson_

## Summary

Trailing ellipses in objects defined in `typing.TYPE_CHECKING` might be meaningful (it might be declaring a stub). Thus, we should skip the `unnecessary-placeholder` (`PIE970`) rule in such contexts.

Closes #10358.

## Test Plan

`cargo nextest run`


---

_Marked ready for review by @tjkuson on 2024-03-14 21:55_

---

_Comment by @github-actions[bot] on 2024-03-14 22:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-03-15 03:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_placeholder.rs`:90 on 2024-03-15 03:09_

Should this be down at the `in_protocol_or_abstract_method` line? (I.e., shouldn't this only apply to ellipses, not stubs?)

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-15 03:09_

---

_@charliermarsh approved on 2024-03-15 03:49_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-03-15 03:49_

---

_Merged by @charliermarsh on 2024-03-15 03:55_

---

_Closed by @charliermarsh on 2024-03-15 03:55_

---

_Branch deleted on 2024-03-15 08:47_

---
