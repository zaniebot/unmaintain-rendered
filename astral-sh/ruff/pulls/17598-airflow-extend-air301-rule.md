```yaml
number: 17598
title: "[`airflow`] Extend `AIR301` rule"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR311
created_at: 2025-04-24T00:44:05Z
updated_at: 2025-04-25T16:49:33Z
url: https://github.com/astral-sh/ruff/pull/17598
synced_at: 2026-01-12T15:56:02Z
```

# [`airflow`] Extend `AIR301` rule

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add "airflow.operators.python.get_current_context" → "airflow.sdk.get_current_context" rule

## Test Plan

<!-- How was it tested? -->

the test fixture has been updated accordingly


---

_Renamed from "feat(AIR301): add "airflow.operators.python.get_current_context" → "airflow.sdk.get_current_context" rule" to "[`airflow`] Extend `AIR301` rule" by @Lee-W on 2025-04-24 00:44_

---

_Comment by @github-actions[bot] on 2025-04-24 00:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2025-04-25 00:51_

---

_Label `rule` added by @ntBre on 2025-04-25 16:48_

---

_Label `preview` added by @ntBre on 2025-04-25 16:48_

---

_@ntBre approved on 2025-04-25 16:49_

---

_Merged by @ntBre on 2025-04-25 16:49_

---

_Closed by @ntBre on 2025-04-25 16:49_

---
