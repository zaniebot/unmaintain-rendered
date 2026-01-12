```yaml
number: 20202
title: "[`airflow`] Convert `DatasetOrTimeSchedule(datasets=...)` to `AssetOrTimeSchedule(assets=...)` (`AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR311-rule
created_at: 2025-09-02T02:23:32Z
updated_at: 2025-09-03T14:12:11Z
url: https://github.com/astral-sh/ruff/pull/20202
synced_at: 2026-01-12T15:56:56Z
```

# [`airflow`] Convert `DatasetOrTimeSchedule(datasets=...)` to `AssetOrTimeSchedule(assets=...)` (`AIR311`)

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

update the argument `datasets` as `assets`

## Test Plan

<!-- How was it tested? -->

update fixture accordingly


---

_Comment by @github-actions[bot] on 2025-09-02 02:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6dd73d2fe268820fd30550344717d3817ab26da5/providers/openlineage/tests/system/openlineage/example_openlineage_schedule_asset_or_time_dag.py#L53'>providers/openlineage/tests/system/openlineage/example_openlineage_schedule_asset_or_time_dag.py:53:9:</a> AIR311 [*] `datasets` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6dd73d2fe268820fd30550344717d3817ab26da5/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L683'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:683:13:</a> AIR311 [*] `datasets` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6dd73d2fe268820fd30550344717d3817ab26da5/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L1242'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:1242:17:</a> AIR311 [*] `datasets` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-09-03 14:09_

---

_Label `preview` added by @ntBre on 2025-09-03 14:09_

---

_@ntBre approved on 2025-09-03 14:11_

Thanks!

---

_Renamed from "[`airflow`] add rule `DatasetOrTimeSchedule(datasets=...)` to `AssetOrTimeSchedule(assets=...)` (`AIR311`)" to "[`airflow`] Convert `DatasetOrTimeSchedule(datasets=...)` to `AssetOrTimeSchedule(assets=...)` (`AIR311`)" by @ntBre on 2025-09-03 14:11_

---

_Merged by @ntBre on 2025-09-03 14:12_

---

_Closed by @ntBre on 2025-09-03 14:12_

---
