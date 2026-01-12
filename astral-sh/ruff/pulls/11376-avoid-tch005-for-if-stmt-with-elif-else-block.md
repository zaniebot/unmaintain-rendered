```yaml
number: 11376
title: "Avoid `TCH005` for `if` stmt with `elif`/`else` block"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/tch005-fix
created_at: 2024-05-12T09:19:46Z
updated_at: 2024-05-13T00:22:26Z
url: https://github.com/astral-sh/ruff/pull/11376
synced_at: 2026-01-12T15:55:37Z
```

# Avoid `TCH005` for `if` stmt with `elif`/`else` block

---

_@dhruvmanila_

## Summary

This PR fixes a bug where the auto-fix for `TCH005` would delete the entire `if` statement.

The fix in this PR is to not consider it a violation if there are any `elif`/`else` blocks. This also matches the behavior of the original plugin.

fixes: #11368 

## Test plan

Add test cases.

---

_Label `bug` added by @dhruvmanila on 2024-05-12 09:19_

---

_Comment by @github-actions[bot] on 2024-05-12 09:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-13 00:22_

---

_Merged by @charliermarsh on 2024-05-13 00:22_

---

_Closed by @charliermarsh on 2024-05-13 00:22_

---

_Branch deleted on 2024-05-13 00:22_

---
