```yaml
number: 9703
title: "[`flake8-async`] Take `pathlib.Path` into account when analyzing async functions"
type: pull_request
state: merged
author: mikeleppane
labels:
  - bug
assignees: []
merged: true
base: main
head: fix(ASYNC101)/include-pathlib-Path-when-analyzing-open-calls
created_at: 2024-01-30T14:19:40Z
updated_at: 2024-01-30T17:50:28Z
url: https://github.com/astral-sh/ruff/pull/9703
synced_at: 2026-01-12T15:55:30Z
```

# [`flake8-async`] Take `pathlib.Path` into account when analyzing async functions

---

_@mikeleppane_

## Summary

This review contains a fix for [ASYNC101](https://docs.astral.sh/ruff/rules/open-sleep-or-subprocess-in-async-function/) (open-sleep-or-subprocess-in-async-function)

The problem is that ruff does not take open calls from pathlib.Path into account in async functions. Path.open() call is still a blocking call. In addition, PTH123 suggests to use pathlib.Path instead of os.open. So this might create an additional confusion.

See: https://github.com/astral-sh/ruff/issues/6892

## Test Plan

```bash
cargo test
```


---

_Comment by @github-actions[bot] on 2024-01-30 14:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-01-30 17:37_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-01-30 17:37_

---

_Renamed from "[`flake8-async`] Take pathlib.Path into account when analyzing open calls in async functions" to "[`flake8-async`] Take `pathlib.Path` into account when analyzing async functions" by @charliermarsh on 2024-01-30 17:38_

---

_Merged by @charliermarsh on 2024-01-30 17:42_

---

_Closed by @charliermarsh on 2024-01-30 17:42_

---
