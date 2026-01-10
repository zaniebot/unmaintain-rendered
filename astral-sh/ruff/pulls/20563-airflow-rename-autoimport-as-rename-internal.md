```yaml
number: 20563
title: "[`airflow`]: rename `AutoImport` as `Rename` (internal)"
type: pull_request
state: merged
author: Lee-W
labels:
  - internal
assignees: []
merged: true
base: main
head: rename-airflow-helper-internal
created_at: 2025-09-25T01:41:37Z
updated_at: 2025-09-30T19:56:27Z
url: https://github.com/astral-sh/ruff/pull/20563
synced_at: 2026-01-10T17:40:28Z
```

# [`airflow`]: rename `AutoImport` as `Rename` (internal)

---

_Pull request opened by @Lee-W on 2025-09-25 01:41_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Since we are trying to import both `AutoImport` and `SourceModuleMoved`, the previous naming was not as descriptive. Renaming it to `Rename` better reflects the intention.

## Test Plan

<!-- How was it tested? -->

no functionality change


---

_Comment by @github-actions[bot] on 2025-09-25 01:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2025-09-25 23:48_

---

_Renamed from "refactor(airflow): rename AutoImport as Rename" to "[`airflow`]: rename `AutoImport` as `Rename` (`internal`)" by @Lee-W on 2025-09-25 23:49_

---

_Renamed from "[`airflow`]: rename `AutoImport` as `Rename` (`internal`)" to "[`airflow`]: rename `AutoImport` as `Rename` (internal)" by @Lee-W on 2025-09-25 23:49_

---

_Label `internal` added by @MichaReiser on 2025-09-26 07:07_

---

_@ntBre approved on 2025-09-30 19:56_

Makes sense to me, thanks!

---

_Merged by @ntBre on 2025-09-30 19:56_

---

_Closed by @ntBre on 2025-09-30 19:56_

---
