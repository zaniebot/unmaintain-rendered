```yaml
number: 18366
title: "[`airflow`] Add unsafe fix for module moved cases (`AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: apply-autofix-to-AIR311
created_at: 2025-05-29T10:26:38Z
updated_at: 2025-05-30T13:27:14Z
url: https://github.com/astral-sh/ruff/pull/18366
synced_at: 2026-01-12T15:56:17Z
```

# [`airflow`] Add unsafe fix for module moved cases (`AIR311`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Follow up on https://github.com/astral-sh/ruff/pull/18093 and apply it to AIR311

---

Rules fixed
* `airflow.models.datasets.expand_alias_to_datasets` → `airflow.models.asset.expand_alias_to_assets`
* `airflow.models.baseoperatorlink.BaseOperatorLink` → `airflow.sdk.BaseOperatorLink`


## Test Plan

<!-- How was it tested? -->
The existing test fixtures have been updated

---

_Closed by @Lee-W on 2025-05-29 11:52_

---

_Reopened by @Lee-W on 2025-05-29 11:52_

---

_Comment by @github-actions[bot] on 2025-05-29 11:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2025-05-30 02:59_

---

_Label `rule` added by @ntBre on 2025-05-30 13:23_

---

_Label `preview` added by @ntBre on 2025-05-30 13:23_

---

_@ntBre approved on 2025-05-30 13:26_

---

_Merged by @ntBre on 2025-05-30 13:27_

---

_Closed by @ntBre on 2025-05-30 13:27_

---
