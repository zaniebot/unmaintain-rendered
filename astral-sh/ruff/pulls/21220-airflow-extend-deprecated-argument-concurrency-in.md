```yaml
number: 21220
title: "[`airflow`] extend deprecated argument `concurrency` in `airflow..DAG` (`AIR301`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
assignees: []
merged: true
base: main
head: extend-AIR301
created_at: 2025-11-03T01:46:45Z
updated_at: 2025-11-03T20:20:21Z
url: https://github.com/astral-sh/ruff/pull/21220
synced_at: 2026-01-10T16:53:55Z
```

# [`airflow`] extend deprecated argument `concurrency` in `airflow..DAG` (`AIR301`)

---

_Pull request opened by @Lee-W on 2025-11-03 01:46_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
* extend AIR301 to include deprecated argument `concurrency` in `airflow....DAG`

## Test Plan

<!-- How was it tested? -->

update the existing test fixture in the first commit and then reorganize in the second one


---

_Comment by @github-actions[bot] on 2025-11-03 02:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ashb reviewed on 2025-11-03 15:52_

Changed LGTM from the airflow side.

---

_Label `rule` added by @ntBre on 2025-11-03 20:19_

---

_@ntBre approved on 2025-11-03 20:20_

Looks good to me, thanks!

---

_Merged by @ntBre on 2025-11-03 20:20_

---

_Closed by @ntBre on 2025-11-03 20:20_

---
