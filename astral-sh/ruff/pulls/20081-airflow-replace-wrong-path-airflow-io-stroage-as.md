```yaml
number: 20081
title: "[`airflow`] replace wrong path `airflow.io.stroage` as `airflow.io.store` (`AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: AIR311-fix
created_at: 2025-08-25T14:30:30Z
updated_at: 2025-08-25T15:15:35Z
url: https://github.com/astral-sh/ruff/pull/20081
synced_at: 2026-01-10T17:46:21Z
```

# [`airflow`] replace wrong path `airflow.io.stroage` as `airflow.io.store` (`AIR311`)

---

_Pull request opened by @Lee-W on 2025-08-25 14:30_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`airflow.io.storage` is not the correct path. it should be `airflow.io.store` instead

## Test Plan

<!-- How was it tested? -->

 the test fixture has been updated accordingly


---

_Renamed from "fix(AIR311): fix airflow.io.storage as airflow.io.store" to "[`airflow`] replace wrong path `airflow.io.stroage` as `airflow.io.store` (`AIR311`)" by @Lee-W on 2025-08-25 14:32_

---

_Comment by @github-actions[bot] on 2025-08-25 14:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @dylwil3 on 2025-08-25 15:14_

---

_Label `rule` added by @dylwil3 on 2025-08-25 15:14_

---

_Label `preview` added by @dylwil3 on 2025-08-25 15:15_

---

_@dylwil3 approved on 2025-08-25 15:15_

Thanks!

---

_Merged by @dylwil3 on 2025-08-25 15:15_

---

_Closed by @dylwil3 on 2025-08-25 15:15_

---
