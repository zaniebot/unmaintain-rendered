```yaml
number: 20695
title: "[`flake8-bugbear`] Mark `B905` and `B912` fixes as unsafe"
type: pull_request
state: merged
author: terror
labels:
  - fixes
assignees: []
merged: true
base: main
head: B905-B912-unsafe
created_at: 2025-10-04T01:04:02Z
updated_at: 2025-10-07T20:55:56Z
url: https://github.com/astral-sh/ruff/pull/20695
synced_at: 2026-01-10T17:34:34Z
```

# [`flake8-bugbear`] Mark `B905` and `B912` fixes as unsafe

---

_Pull request opened by @terror on 2025-10-04 01:04_

Resolves https://github.com/astral-sh/ruff/issues/20694

This PR updates the `zip_without_explicit_strict` and `map_without_explicit_strict` rules so their fixes are always marked unsafe, following Brent's guidance that adding `strict=False` can silently preserve buggy behaviour when inputs differ. The fix safety docs now spell out that reasoning, the applicability drops to `Unsafe`, and the snapshots were refreshed so Ruff clearly warns users before applying the edit.

---

_Comment by @github-actions[bot] on 2025-10-04 01:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -176 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -176 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1582'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1582:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1582'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1582:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1725'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1725:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1725'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1725:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1899'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1899:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1899'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1899:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1932'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1932:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1932'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1932:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 `zip()` without an explicit `strict=` parameter
... 126 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B905 | 176 | 0 | 0 | 0 | 176 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -176 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -176 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1582'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1582:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1582'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1582:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1725'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1725:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1725'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1725:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1899'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1899:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1899'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1899:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1932'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1932:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1932'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1932:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/6a71094fb3377ce0323014077e702f123c71846d/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 `zip()` without an explicit `strict=` parameter
... 126 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B905 | 176 | 0 | 0 | 0 | 176 |

</p>
</details>




---

_Label `fixes` added by @ntBre on 2025-10-07 20:54_

---

_@ntBre approved on 2025-10-07 20:54_

Thank you!

---

_Renamed from "Mark `B905` and `B912` strict fixes as unsafe" to "[`flake8-bugbear`] Mark `B905` and `B912` fixes as unsafe" by @ntBre on 2025-10-07 20:55_

---

_Merged by @ntBre on 2025-10-07 20:55_

---

_Closed by @ntBre on 2025-10-07 20:55_

---
