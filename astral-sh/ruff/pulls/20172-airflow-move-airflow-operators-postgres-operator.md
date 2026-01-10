```yaml
number: 20172
title: "[`airflow`] Move `airflow.operators.postgres_operator.Mapping` from `AIR302` to `AIR301`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: move-provider-no-replacement-to-removal-in-3
created_at: 2025-08-31T01:32:57Z
updated_at: 2025-09-03T14:18:17Z
url: https://github.com/astral-sh/ruff/pull/20172
synced_at: 2026-01-10T17:46:21Z
```

# [`airflow`] Move `airflow.operators.postgres_operator.Mapping` from `AIR302` to `AIR301`

---

_Pull request opened by @Lee-W on 2025-08-31 01:32_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

### Why
Removal should be grouped into the same category. It doesn't matter whether it's from a provider or not (and the only case we used to have was not anyway).
`ProviderReplacement` is used to indicate that we have a replacement and we might need to install an extra Python package to cater to it.

### What
Move `airflow.operators.postgres_operator.Mapping` from AIR302 to AIR301 and get rid of `ProviderReplace::None`

## Test Plan

<!-- How was it tested? -->

Update the test fixtures accordingly in the first commit and reorganize them in the second commit


---

_Renamed from "feat(AIR3): Replace ProviderReplacement::None as Replacement::None" to "[`airflow`] Move airflow.operators.postgres_operator.Mapping from AIR302 to AIR301 (`AIR301`, `AIR302`)" by @Lee-W on 2025-08-31 01:37_

---

_Comment by @github-actions[bot] on 2025-08-31 01:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-09-03 14:15_

---

_Label `preview` added by @ntBre on 2025-09-03 14:15_

---

_@ntBre approved on 2025-09-03 14:17_

Nice!

---

_Renamed from "[`airflow`] Move airflow.operators.postgres_operator.Mapping from AIR302 to AIR301 (`AIR301`, `AIR302`)" to "[`airflow`] Move `airflow.operators.postgres_operator.Mapping` from `AIR302` to `AIR301`" by @ntBre on 2025-09-03 14:18_

---

_Merged by @ntBre on 2025-09-03 14:18_

---

_Closed by @ntBre on 2025-09-03 14:18_

---
