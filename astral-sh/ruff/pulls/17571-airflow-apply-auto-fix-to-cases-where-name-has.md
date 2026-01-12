```yaml
number: 17571
title: "[`airflow`] Apply auto fix to cases where name has been changed in Airflow 3 (`AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: auto-import-AIR311
created_at: 2025-04-23T02:43:55Z
updated_at: 2025-04-24T19:48:54Z
url: https://github.com/astral-sh/ruff/pull/17571
synced_at: 2026-01-12T15:56:02Z
```

# [`airflow`] Apply auto fix to cases where name has been changed in Airflow 3 (`AIR311`)

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

Apply auto fix to cases where the name has been changed in Airflow 3 (`AIR311`)

## Test Plan

<!-- How was it tested? -->

The test features has been updated


---

_Comment by @github-actions[bot] on 2025-04-23 02:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +12 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:36:</a> AIR311 [*] `airflow.datasets.DatasetAny` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:36:</a> AIR311 `airflow.datasets.DatasetAny` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:48:</a> AIR311 [*] `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:48:</a> AIR311 `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L355'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:355:67:</a> AIR311 [*] `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L355'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:355:67:</a> AIR311 `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L678'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:678:18:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L678'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:678:18:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L774'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:774:18:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L774'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:774:18:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L949'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:949:22:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/b01e534195fff5dea03d000051e6469b83852f6c/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L949'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:949:22:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 12 | 0 | 0 | 12 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-04-24 18:10_

---

_Label `preview` added by @ntBre on 2025-04-24 18:10_

---

_@ntBre approved on 2025-04-24 18:13_

Looks great, thanks! I'll merge today after the ongoing release.

---

_Merged by @ntBre on 2025-04-24 19:48_

---

_Closed by @ntBre on 2025-04-24 19:48_

---
