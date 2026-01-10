```yaml
number: 11312
title: "[`flake8-bugbear`] Ignore enum classes in `cached-instance-method` (`B019`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/en
created_at: 2024-05-06T17:58:41Z
updated_at: 2024-05-06T18:19:23Z
url: https://github.com/astral-sh/ruff/pull/11312
synced_at: 2026-01-10T22:37:02Z
```

# [`flake8-bugbear`] Ignore enum classes in `cached-instance-method` (`B019`)

---

_Pull request opened by @charliermarsh on 2024-05-06 17:58_

## Summary

While I was here, I also updated the rule to use `function_type::classify` rather than hard-coding `staticmethod` and friends.

Per Carl:

> Enum instances are already referred to by the class, forming a cycle that won't get collected until the class itself does. At which point the `lru_cache` itself would be collected, too.

Closes https://github.com/astral-sh/ruff/issues/9912.


---

_Label `rule` added by @charliermarsh on 2024-05-06 18:00_

---

_Comment by @github-actions[bot] on 2024-05-06 18:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-05-06 18:19_

---

_Closed by @charliermarsh on 2024-05-06 18:19_

---

_Branch deleted on 2024-05-06 18:19_

---
