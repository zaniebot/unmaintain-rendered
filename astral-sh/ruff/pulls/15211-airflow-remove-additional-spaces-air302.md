```yaml
number: 15211
title: "[`airflow`] Remove additional spaces (`AIR302`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-AIR302-typo
created_at: 2024-12-31T07:32:40Z
updated_at: 2025-01-23T08:50:10Z
url: https://github.com/astral-sh/ruff/pull/15211
synced_at: 2026-01-10T20:05:43Z
```

# [`airflow`] Remove additional spaces (`AIR302`)

---

_Pull request opened by @Lee-W on 2024-12-31 07:32_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

During https://github.com/astral-sh/ruff/pull/15209, additional spaces was accidentally added to the rule `airflow.operators.latest_only.LatestOnlyOperator`. This PR fixes this issue

## Test Plan

<!-- How was it tested? -->

A test fixture has been included for the rule.


---

_Marked ready for review by @Lee-W on 2024-12-31 07:34_

---

_Comment by @github-actions[bot] on 2024-12-31 07:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-12-31 08:27_

Good catch, thanks!

---

_Label `internal` added by @dhruvmanila on 2024-12-31 08:27_

---

_Renamed from "[airflow]: remove additional spaces (AIR302)" to "[`airflow`] Remove additional spaces (`AIR302`)" by @dhruvmanila on 2024-12-31 08:28_

---

_Merged by @dhruvmanila on 2024-12-31 08:28_

---

_Closed by @dhruvmanila on 2024-12-31 08:28_

---

_Branch deleted on 2025-01-23 08:50_

---
