```yaml
number: 16978
title: "[`airflow`] Move `AIR301` to `AIR002`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: move-AIR301-to-AIR201
created_at: 2025-03-26T08:53:38Z
updated_at: 2025-04-02T15:07:35Z
url: https://github.com/astral-sh/ruff/pull/16978
synced_at: 2026-01-10T19:40:36Z
```

# [`airflow`] Move `AIR301` to `AIR002`

---

_Pull request opened by @Lee-W on 2025-03-26 08:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Unlike other AIR3XX rules, this best practice can be applied to Airflow 1 and Airflow 2 as well. Thus, we think it might make sense for use to move it to AIR002 so that the first number of the error align to Airflow version as possible to reduce confusion

## Test Plan

<!-- How was it tested? -->

the test fixture has been updated


---

_Comment by @github-actions[bot] on 2025-03-26 09:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+113 -113 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+113 -113 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/src/airflow/models/dag.py#L301'>airflow-core/src/airflow/models/dag.py:301:16:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/src/airflow/models/dag.py#L301'>airflow-core/src/airflow/models/dag.py:301:16:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/integration/executors/test_celery_executor.py#L240'>airflow-core/tests/integration/executors/test_celery_executor.py:240:21:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/integration/executors/test_celery_executor.py#L240'>airflow-core/tests/integration/executors/test_celery_executor.py:240:21:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/integration/executors/test_celery_executor.py#L276'>airflow-core/tests/integration/executors/test_celery_executor.py:276:21:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/integration/executors/test_celery_executor.py#L276'>airflow-core/tests/integration/executors/test_celery_executor.py:276:21:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L283'>airflow-core/tests/unit/dag_processing/test_collection.py:283:15:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L283'>airflow-core/tests/unit/dag_processing/test_collection.py:283:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L369'>airflow-core/tests/unit/dag_processing/test_collection.py:369:15:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L369'>airflow-core/tests/unit/dag_processing/test_collection.py:369:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L387'>airflow-core/tests/unit/dag_processing/test_collection.py:387:15:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L387'>airflow-core/tests/unit/dag_processing/test_collection.py:387:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L481'>airflow-core/tests/unit/dag_processing/test_collection.py:481:15:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_collection.py#L481'>airflow-core/tests/unit/dag_processing/test_collection.py:481:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L141'>airflow-core/tests/unit/dag_processing/test_processor.py:141:18:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L141'>airflow-core/tests/unit/dag_processing/test_processor.py:141:18:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L167'>airflow-core/tests/unit/dag_processing/test_processor.py:167:18:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L167'>airflow-core/tests/unit/dag_processing/test_processor.py:167:18:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L191'>airflow-core/tests/unit/dag_processing/test_processor.py:191:18:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L191'>airflow-core/tests/unit/dag_processing/test_processor.py:191:18:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L218'>airflow-core/tests/unit/dag_processing/test_processor.py:218:18:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L218'>airflow-core/tests/unit/dag_processing/test_processor.py:218:18:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L305'>airflow-core/tests/unit/dag_processing/test_processor.py:305:11:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L305'>airflow-core/tests/unit/dag_processing/test_processor.py:305:11:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L340'>airflow-core/tests/unit/dag_processing/test_processor.py:340:10:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/dag_processing/test_processor.py#L340'>airflow-core/tests/unit/dag_processing/test_processor.py:340:10:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/jobs/test_scheduler_job.py#L6261'>airflow-core/tests/unit/jobs/test_scheduler_job.py:6261:16:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/jobs/test_scheduler_job.py#L6261'>airflow-core/tests/unit/jobs/test_scheduler_job.py:6261:16:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dag.py#L2180'>airflow-core/tests/unit/models/test_dag.py:2180:15:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dag.py#L2180'>airflow-core/tests/unit/models/test_dag.py:2180:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dag.py#L2189'>airflow-core/tests/unit/models/test_dag.py:2189:15:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dag.py#L2189'>airflow-core/tests/unit/models/test_dag.py:2189:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dag.py#L2879'>airflow-core/tests/unit/models/test_dag.py:2879:10:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dag.py#L2879'>airflow-core/tests/unit/models/test_dag.py:2879:10:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dagbag.py#L934'>airflow-core/tests/unit/models/test_dagbag.py:934:14:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/models/test_dagbag.py#L934'>airflow-core/tests/unit/models/test_dagbag.py:934:14:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/serialization/test_serialized_objects.py#L143'>airflow-core/tests/unit/serialization/test_serialized_objects.py:143:18:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/serialization/test_serialized_objects.py#L143'>airflow-core/tests/unit/serialization/test_serialized_objects.py:143:18:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/serialization/test_serialized_objects.py#L238'>airflow-core/tests/unit/serialization/test_serialized_objects.py:238:21:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/airflow-core/tests/unit/serialization/test_serialized_objects.py#L238'>airflow-core/tests/unit/serialization/test_serialized_objects.py:238:21:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/devel-common/src/tests_common/pytest_plugin.py#L1929'>devel-common/src/tests_common/pytest_plugin.py:1929:19:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/devel-common/src/tests_common/pytest_plugin.py#L1929'>devel-common/src/tests_common/pytest_plugin.py:1929:19:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/devel-common/src/tests_common/pytest_plugin.py#L2054'>devel-common/src/tests_common/pytest_plugin.py:2054:19:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/devel-common/src/tests_common/pytest_plugin.py#L2054'>devel-common/src/tests_common/pytest_plugin.py:2054:19:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/providers/apache/kafka/tests/system/apache/kafka/example_dag_event_listener.py#L85'>providers/apache/kafka/tests/system/apache/kafka/example_dag_event_listener.py:85:6:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/providers/apache/kafka/tests/system/apache/kafka/example_dag_event_listener.py#L85'>providers/apache/kafka/tests/system/apache/kafka/example_dag_event_listener.py:85:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/providers/arangodb/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L25'>providers/arangodb/src/airflow/providers/arangodb/example_dags/example_arangodb.py:25:7:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/providers/arangodb/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L25'>providers/arangodb/src/airflow/providers/arangodb/example_dags/example_arangodb.py:25:7:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/providers/asana/tests/system/asana/example_asana.py#L49'>providers/asana/tests/system/asana/example_asana.py:49:6:</a> AIR002 DAG should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/4b06e9a60dec7787feb01711881ed4562fbb1ec3/providers/asana/tests/system/asana/example_asana.py#L49'>providers/asana/tests/system/asana/example_asana.py:49:6:</a> AIR301 DAG should have an explicit `schedule` argument
... 176 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR002 | 113 | 113 | 0 | 0 | 0 |
| AIR301 | 113 | 0 | 113 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-03-26 16:14_

---

_Label `preview` added by @ntBre on 2025-03-26 16:14_

---

_Comment by @uranusjr on 2025-03-27 06:06_

Arguably this even applies to 1.x? Although nobody in their right minds should care about 1.x in 2025. What I’m trying to say is, maybe this makes even more sense as AIR002?

---

_Comment by @Lee-W on 2025-03-27 06:07_

> Arguably this even applies to 1.x? Although nobody in their right minds should care about 1.x in 2025. What I’m trying to say is, maybe this makes even more sense as AIR002?

I thought we're using `schedule_interval` in 1.x?

---

_Comment by @Lee-W on 2025-03-27 06:08_

> > Arguably this even applies to 1.x? Although nobody in their right minds should care about 1.x in 2025. What I’m trying to say is, maybe this makes even more sense as AIR002?
> 
> I thought we're using `schedule_interval` in 1.x?

oh, looking into the code again. yep. `schedule_interval` is checked as well.

---

_Comment by @Lee-W on 2025-03-27 06:10_

probably need to tweak the description a bit. 

---

_Renamed from "[airflow] move AIR301 to AIR201" to "[airflow] move AIR301 to AIR102" by @Lee-W on 2025-03-27 13:39_

---

_Renamed from "[airflow] move AIR301 to AIR102" to "[airflow] move AIR301 to AIR002" by @Lee-W on 2025-03-27 13:40_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:15 on 2025-03-31 20:30_

"The default value of the `schedule` parameter on ..."

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:17 on 2025-03-31 20:31_

"Airflow 3 changed the default value to `None`, ..."

---

_@dhruvmanila approved on 2025-03-31 20:35_

Thanks!

---

_Renamed from "[airflow] move AIR301 to AIR002" to "[`airflow`] Move `AIR301` to `AIR002`" by @dhruvmanila on 2025-03-31 20:36_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:15 on 2025-04-01 03:03_

Updated. Thanks!

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:17 on 2025-04-01 03:03_

Updated. Thanks!

---

_@Lee-W reviewed on 2025-04-01 03:03_

---

_Merged by @dhruvmanila on 2025-04-02 15:07_

---

_Closed by @dhruvmanila on 2025-04-02 15:07_

---
