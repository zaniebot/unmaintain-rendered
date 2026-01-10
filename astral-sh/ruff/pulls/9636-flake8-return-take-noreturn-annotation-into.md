```yaml
number: 9636
title: "[`flake8-return`] Take `NoReturn` annotation into account when analyzing implicit returns"
type: pull_request
state: merged
author: mikeleppane
labels:
  - bug
assignees: []
merged: true
base: main
head: fix(RET503)/respect_noreturn_annotation
created_at: 2024-01-24T16:23:53Z
updated_at: 2024-01-24T17:28:07Z
url: https://github.com/astral-sh/ruff/pull/9636
synced_at: 2026-01-10T22:57:09Z
```

# [`flake8-return`] Take `NoReturn` annotation into account when analyzing implicit returns

---

_Pull request opened by @mikeleppane on 2024-01-24 16:23_

## Summary

When we are analyzing the implicit return rule this change add an additional check to verify if the call expression has been annotated with NoReturn type from typing module.

See: https://github.com/astral-sh/ruff/issues/5474

## Test Plan

```bash
cargo test
```

---

_Comment by @github-actions[bot] on 2024-01-24 16:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-01-24 17:14_

Thx!

---

_Label `bug` added by @charliermarsh on 2024-01-24 17:15_

---

_Renamed from "fix(RET503): take NoReturn function annotation into account when analyzing implicit returns" to "[`flake8-return`] Take `NoReturn` annotation into account when analyzing implicit returns" by @charliermarsh on 2024-01-24 17:15_

---

_Merged by @charliermarsh on 2024-01-24 17:19_

---

_Closed by @charliermarsh on 2024-01-24 17:19_

---
