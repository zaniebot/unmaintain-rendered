```yaml
number: 15525
title: " [airflow] extend and fix AIR302 rules"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-air302-rules
created_at: 2025-01-16T07:49:38Z
updated_at: 2025-01-23T08:50:03Z
url: https://github.com/astral-sh/ruff/pull/15525
synced_at: 2026-01-10T20:05:43Z
```

#  [airflow] extend and fix AIR302 rules

---

_Pull request opened by @Lee-W on 2025-01-16 07:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

* fix
    * `airflow.secrets.local_filesystem.get_connection` should be `airflow.secrets.local_filesystemLocalFilesystemBackend.get_connection` → `airflow.secrets.local_filesystemLocalFilesystemBackend.get_connections`
    * `airflow.utils.dates.date_range` does not have a replacement
* feat
    * `airflow.utils.dag_parsing_context.get_parsing_context` -> `airflow.sdk.get_parsing_context`


## Test Plan

<!-- How was it tested? -->
a test fixture has been updated

---

_Comment by @github-actions[bot] on 2025-01-16 08:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @Lee-W on 2025-01-16 08:28_

---

_Renamed from "fix(AIR302): fix wrong airflow.utils.dates.date_range replacement" to " [airflow] extend and fix AIR302 rules" by @Lee-W on 2025-01-16 09:08_

---

_Marked ready for review by @Lee-W on 2025-01-16 09:09_

---

_Label `rule` added by @MichaReiser on 2025-01-16 09:39_

---

_Merged by @MichaReiser on 2025-01-16 09:40_

---

_Closed by @MichaReiser on 2025-01-16 09:40_

---

_Label `preview` added by @MichaReiser on 2025-01-16 09:40_

---

_Branch deleted on 2025-01-23 08:50_

---
