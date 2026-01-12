```yaml
number: 20250
title: Stabilize the remaining Airflow rules
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: brent/airflow
created_at: 2025-09-04T19:46:32Z
updated_at: 2025-09-08T14:01:14Z
url: https://github.com/astral-sh/ruff/pull/20250
synced_at: 2026-01-12T15:56:57Z
```

# Stabilize the remaining Airflow rules

---

_@ntBre_

- **Stabilize `airflow3-suggested-update` (`AIR311`)**
- **Stabilize `airflow3-suggested-to-move-to-provider` (`AIR312`)**
- **Stabilize `airflow3-removal` (`AIR301`)**
- **Stabilize `airflow3-moved-to-provider` (`AIR302`)**
- **Stabilize `airflow-dag-no-schedule-argument` (`AIR002`)**

I put this all in one PR to make it easier to double check with @Lee-W before we merge this. I also made a few minor documentation changes and updated one error message that I want to make sure are okay. But for the most part this just moves the rules from `RuleGroup::Preview` to `RuleGroup::Stable`!

Fixes #17749 

---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 19:46_

---

_Label `rule` added by @ntBre on 2025-09-04 19:46_

---

_Comment by @github-actions[bot] on 2025-09-04 19:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2532 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2532 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L115'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:115:17:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L125'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:125:26:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L154'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:154:18:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L193'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:193:17:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L193'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:193:52:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L232'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:232:16:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L72'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:72:40:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L92'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:92:40:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/security.py#L244'>airflow-core/src/airflow/api_fastapi/core_api/security.py:244:21:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/api_fastapi/core_api/security.py#L281'>airflow-core/src/airflow/api_fastapi/core_api/security.py:281:21:</a> AIR311 `airflow.models.Variable` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/src/airflow/cli/commands/connection_command.py#L114'>airflow-core/src/airflow/cli/commands/connection_command.py:114:37:</a> AIR311 `airflow.models.Connection` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 2298 additional changes omitted for rule AIR311
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_xcom.py#L406'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_xcom.py:406:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_collection.py#L360'>airflow-core/tests/unit/dag_processing/test_collection.py:360:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_collection.py#L444'>airflow-core/tests/unit/dag_processing/test_collection.py:444:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_collection.py#L469'>airflow-core/tests/unit/dag_processing/test_collection.py:469:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_collection.py#L683'>airflow-core/tests/unit/dag_processing/test_collection.py:683:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_collection.py#L726'>airflow-core/tests/unit/dag_processing/test_collection.py:726:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_processor.py#L1006'>airflow-core/tests/unit/dag_processing/test_processor.py:1006:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_processor.py#L1054'>airflow-core/tests/unit/dag_processing/test_processor.py:1054:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_processor.py#L1097'>airflow-core/tests/unit/dag_processing/test_processor.py:1097:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_processor.py#L1157'>airflow-core/tests/unit/dag_processing/test_processor.py:1157:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/airflow-core/tests/unit/dag_processing/test_processor.py#L1217'>airflow-core/tests/unit/dag_processing/test_processor.py:1217:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
... 193 additional changes omitted for rule AIR002
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/dev/airflow_perf/dags/perf_dag_1.py#L41'>dev/airflow_perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/dev/airflow_perf/dags/perf_dag_1.py#L48'>dev/airflow_perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/dev/airflow_perf/scheduler_dag_execution_timing.py#L255'>dev/airflow_perf/scheduler_dag_execution_timing.py:255:10:</a> AIR301 `airflow.utils.db.create_session` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/dev/airflow_perf/scheduler_dag_execution_timing.py#L302'>dev/airflow_perf/scheduler_dag_execution_timing.py:302:18:</a> AIR301 `airflow.utils.db.create_session` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/performance/src/performance_dags/performance_dag/performance_dag.py#L100'>performance/src/performance_dags/performance_dag/performance_dag.py:100:13:</a> AIR312 `airflow.operators.bash.BashOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/performance/src/performance_dags/performance_dag/performance_dag.py#L116'>performance/src/performance_dags/performance_dag/performance_dag.py:116:13:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/performance/src/performance_dags/performance_dag/performance_dag.py#L249'>performance/src/performance_dags/performance_dag/performance_dag.py:249:9:</a> AIR301 [*] `schedule_interval` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py#L189'>providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py:189:67:</a> AIR301 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L421'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:421:13:</a> AIR301 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/amazon/tests/unit/amazon/aws/log/test_s3_task_handler.py#L349'>providers/amazon/tests/unit/amazon/aws/log/test_s3_task_handler.py:349:70:</a> AIR301 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:32:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:39:5:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L63'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:63:17:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L67'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:67:17:</a> AIR301 [*] `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/4d1d3183c9d36f94db2605cbbd890143c552b126/providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py#L885'>providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py:885:13:</a> AIR301 `filename_template` is removed in Airflow 3.0
... 7 additional changes omitted for rule AIR301
... 2495 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 2308 | 2308 | 0 | 0 | 0 |
| AIR002 | 203 | 203 | 0 | 0 | 0 |
| AIR301 | 17 | 17 | 0 | 0 | 0 |
| AIR302 | 2 | 2 | 0 | 0 | 0 |
| AIR312 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-09-04 19:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:61 on 2025-09-04 19:59_

This is the largest change I made. The second sentence "It still works..." made it sound like _removed_ wasn't exactly the right word. I'm happy to revert this if preferred, though.

---

_Marked ready for review by @ntBre on 2025-09-04 19:59_

---

_@Lee-W reviewed on 2025-09-05 02:14_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:61 on 2025-09-05 02:14_

It's technically been removed. 👀 We added an alias to the new place instead.

something like https://github.com/apache/airflow/blob/b9937fbbb3243b5ea25b7091bb0394cb8c238761/airflow-core/src/airflow/datasets/__init__.py#L32-L61

I like to `remove` a bit more. But I'm good with `deprecated` if it still makes more sense to you. 🙂

---

_Comment by @Lee-W on 2025-09-05 02:15_

also kinda wonder if the description here will also be updated. Thanks!

<img width="1334" height="768" alt="image" src="https://github.com/user-attachments/assets/320814ce-f72d-40bb-90f3-918270125c47" />


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:61 on 2025-09-05 02:22_

Ahh okay, I misunderstood. I'll put this back to `removed`. Thank you!

---

_@ntBre reviewed on 2025-09-05 02:22_

---

_Comment by @ntBre on 2025-09-05 02:28_

The documentation on the website is all generated from the docstrings and `message` implementations, so that should update to reflect whatever we change here after the release. Hmm, but it doesn't look like the AIR002 entry in that table updated after the patch release today. I'll look into that too.

---

_Comment by @Lee-W on 2025-09-05 03:11_

I think AIR302, AIR312 should probably be updated as well 🤔 let me know if there's anything I can help

---

_Review requested from @dylwil3 by @ntBre on 2025-09-05 21:48_

---

_Comment by @ntBre on 2025-09-05 21:49_

The docs website should now be up to date! There was just a temporary issue in the release deployment yesterday.

---

_@dylwil3 approved on 2025-09-08 13:59_

---

_Merged by @ntBre on 2025-09-08 14:01_

---

_Closed by @ntBre on 2025-09-08 14:01_

---

_Branch deleted on 2025-09-08 14:01_

---
