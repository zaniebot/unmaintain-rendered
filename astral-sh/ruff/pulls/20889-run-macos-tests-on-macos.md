```yaml
number: 20889
title: Run macos tests on macos
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/macos-test
created_at: 2025-10-15T12:23:57Z
updated_at: 2025-10-15T12:46:13Z
url: https://github.com/astral-sh/ruff/pull/20889
synced_at: 2026-01-10T17:34:34Z
```

# Run macos tests on macos

---

_Pull request opened by @MichaReiser on 2025-10-15 12:23_

Because, that's sort of the point

---

_Label `ci` added by @MichaReiser on 2025-10-15 12:23_

---

_@sharkdp approved on 2025-10-15 12:31_

Who approved that original PR? Shame on them.

---

_Merged by @MichaReiser on 2025-10-15 12:41_

---

_Closed by @MichaReiser on 2025-10-15 12:41_

---

_Branch deleted on 2025-10-15 12:41_

---

_Comment by @github-actions[bot] on 2025-10-15 12:46_

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
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1639'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1639:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1639'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1639:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1782'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1782:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1782'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1782:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1956'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1956:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1956'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1956:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1989'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1989:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1989'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1989:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 `zip()` without an explicit `strict=` parameter
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
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L83'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:83:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L84'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:84:41:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py#L325'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_event_logs.py:325:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py#L106'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_import_error.py:106:56:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1639'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1639:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1639'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1639:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1782'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1782:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1782'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1782:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1956'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1956:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1956'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1956:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1989'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1989:26:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1989'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1989:26:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/assets/test_evaluation.py#L106'>airflow-core/tests/unit/assets/test_evaluation.py:106:79:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/models/test_dagrun.py#L2493'>airflow-core/tests/unit/models/test_dagrun.py:2493:22:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L114'>airflow-core/tests/unit/timetables/test_events_timetable.py:114:27:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_events_timetable.py#L69'>airflow-core/tests/unit/timetables/test_events_timetable.py:69:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-core/tests/unit/timetables/test_workday_timetable.py#L57'>airflow-core/tests/unit/timetables/test_workday_timetable.py:57:10:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L254'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:254:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L263'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:263:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L269'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:269:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py#L275'>airflow-ctl/tests/airflow_ctl/ctl/test_cli_config.py:275:42:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/airflow_perf/sql_queries.py#L108'>dev/airflow_perf/sql_queries.py:108:21:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2945'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2945:31:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/src/airflow_breeze/utils/packages.py#L1166'>dev/breeze/src/airflow_breeze/utils/packages.py:1166:54:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L61'>dev/breeze/tests/test_selective_checks.py:61:32:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/breeze/tests/test_selective_checks.py#L64'>dev/breeze/tests/test_selective_checks.py:64:50:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/stats/get_important_pr_candidates.py#L677'>dev/stats/get_important_pr_candidates.py:677:36:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/dev/validate_version_added_fields_in_config.py#L126'>dev/validate_version_added_fields_in_config.py:126:35:</a> B905 `zip()` without an explicit `strict=` parameter
- <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 [*] `zip()` without an explicit `strict=` parameter
+ <a href='https://github.com/apache/airflow/blob/7203012b3f3d79f81ea5fd7ef1c9895aa5e260c3/devel-common/src/tests_common/test_utils/logs.py#L168'>devel-common/src/tests_common/test_utils/logs.py:168:48:</a> B905 `zip()` without an explicit `strict=` parameter
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

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
