```yaml
number: 15179
title: "[`flake8-type-checking`] Disable TC006 & TC007 in stub files"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
assignees: []
merged: true
base: main
head: bugfix/tc-rules-in-stubs
created_at: 2024-12-29T07:56:41Z
updated_at: 2025-01-02T11:42:56Z
url: https://github.com/astral-sh/ruff/pull/15179
synced_at: 2026-01-10T20:42:27Z
```

# [`flake8-type-checking`] Disable TC006 & TC007 in stub files

---

_Pull request opened by @Daverball on 2024-12-29 07:56_

Fixes: #15176

## Summary

Neither of these rules make any sense in stub files. Technically TC007 should already not have triggered, due to the typing only context of the binding, but it's better to be explicit.

Keeping TC008 enabled on the other hand makes sense to me, although we could probably be more aggressive with unquoting in a typing runtime context.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-12-29 08:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Daverball on 2024-12-29 08:35_

Addressed the TC008 part in #15180

---

_Merged by @charliermarsh on 2024-12-29 19:39_

---

_Closed by @charliermarsh on 2024-12-29 19:39_

---

_@charliermarsh reviewed on 2024-12-29 19:39_

Thanks!

---

_Label `rule` added by @dhruvmanila on 2025-01-02 11:42_

---
