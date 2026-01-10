```yaml
number: 17540
title: Apply auto import to air311
type: pull_request
state: closed
author: Lee-W
labels: []
assignees: []
draft: true
base: main
head: apply-auto-import-to-AIR311
created_at: 2025-04-22T08:19:01Z
updated_at: 2025-04-23T02:44:14Z
url: https://github.com/astral-sh/ruff/pull/17540
synced_at: 2026-01-10T19:33:02Z
```

# Apply auto import to air311

---

_Pull request opened by @Lee-W on 2025-04-22 08:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-22 08:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +22 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +22 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:</a> AIR301 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py#L40'>providers/common/compat/src/airflow/providers/common/compat/openlineage/utils/utils.py:40:59:</a> AIR301 `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:36:</a> AIR311 [*] `airflow.datasets.DatasetAny` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:36:</a> AIR311 `airflow.datasets.DatasetAny` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:48:</a> AIR311 [*] `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:48:</a> AIR311 `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L355'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:355:67:</a> AIR311 [*] `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L355'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:355:67:</a> AIR311 `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L580'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:580:21:</a> AIR301 [*] `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L580'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:580:21:</a> AIR301 `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L678'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:678:18:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L678'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:678:18:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L774'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:774:18:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L774'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:774:18:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L949'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:949:22:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/493402d24ece8c87b8004fa8a476b232408069cc/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L949'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:949:22:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 12 | 0 | 0 | 12 | 0 |
| AIR301 | 11 | 0 | 1 | 10 | 0 |

</p>
</details>




---

_Closed by @Lee-W on 2025-04-23 02:44_

---
