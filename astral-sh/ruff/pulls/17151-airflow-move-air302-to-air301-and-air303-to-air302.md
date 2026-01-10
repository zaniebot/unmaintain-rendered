```yaml
number: 17151
title: "[`airflow`] Move `AIR302` to `AIR301` and `AIR303` to `AIR302`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: reorganize-AIR-rules
created_at: 2025-04-02T13:13:36Z
updated_at: 2025-04-02T17:31:59Z
url: https://github.com/astral-sh/ruff/pull/17151
synced_at: 2026-01-10T19:40:37Z
```

# [`airflow`] Move `AIR302` to `AIR301` and `AIR303` to `AIR302`

---

_Pull request opened by @Lee-W on 2025-04-02 13:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Following up the discussion in https://github.com/astral-sh/ruff/issues/14626#issuecomment-2766548545, we're to reorganize airflow rules. Before this discussion happens, we combine required changes and suggested changes in to one single error code.

This PR first rename the original error code to the new error code as we discussed. We will gradually extract suggested changes out of AIR301 and AIR302 to AIR311 and AIR312 in the following PRs

## Test Plan

<!-- How was it tested? -->
Except for file, error code rename, the test case should work as it used to be.

---

_Renamed from "Reorganize air rules" to "Rename the original AIR302, AIR303 as AIR301 and AIR302" by @Lee-W on 2025-04-02 13:14_

---

_Comment by @github-actions[bot] on 2025-04-02 13:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+178 -178 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+178 -178 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/decorators/test_bash.py#L518'>airflow-core/tests/unit/decorators/test_bash.py:518:13:</a> AIR302 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/decorators/test_bash.py#L518'>airflow-core/tests/unit/decorators/test_bash.py:518:13:</a> AIR303 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/dags/perf_dag_1.py#L41'>dev/airflow_perf/dags/perf_dag_1.py:41:10:</a> AIR302 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/dags/perf_dag_1.py#L41'>dev/airflow_perf/dags/perf_dag_1.py:41:10:</a> AIR303 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/dags/perf_dag_1.py#L48'>dev/airflow_perf/dags/perf_dag_1.py:48:12:</a> AIR302 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/dags/perf_dag_1.py#L48'>dev/airflow_perf/dags/perf_dag_1.py:48:12:</a> AIR303 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/scheduler_dag_execution_timing.py#L256'>dev/airflow_perf/scheduler_dag_execution_timing.py:256:13:</a> AIR301 `airflow.utils.db.create_session` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/scheduler_dag_execution_timing.py#L256'>dev/airflow_perf/scheduler_dag_execution_timing.py:256:13:</a> AIR302 `airflow.utils.db.create_session` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/scheduler_dag_execution_timing.py#L303'>dev/airflow_perf/scheduler_dag_execution_timing.py:303:21:</a> AIR301 `airflow.utils.db.create_session` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/airflow_perf/scheduler_dag_execution_timing.py#L303'>dev/airflow_perf/scheduler_dag_execution_timing.py:303:21:</a> AIR302 `airflow.utils.db.create_session` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR301 [*] `schedule_interval` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR302 [*] `schedule_interval` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR303 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py#L189'>providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py:189:67:</a> AIR301 `filename_template` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py#L189'>providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py:189:67:</a> AIR302 `filename_template` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py#L41'>providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py:41:19:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py#L41'>providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py:41:19:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_appflow.py#L102'>providers/amazon/tests/system/amazon/aws/example_appflow.py:102:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_appflow.py#L102'>providers/amazon/tests/system/amazon/aws/example_appflow.py:102:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L182'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:182:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L182'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:182:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_athena.py#L159'>providers/amazon/tests/system/amazon/aws/example_athena.py:159:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_athena.py#L159'>providers/amazon/tests/system/amazon/aws/example_athena.py:159:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L65'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:65:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L65'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:65:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_batch.py#L258'>providers/amazon/tests/system/amazon/aws/example_batch.py:258:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_batch.py#L258'>providers/amazon/tests/system/amazon/aws/example_batch.py:258:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L113'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:113:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L113'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:113:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L114'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:114:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L114'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:114:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L146'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:146:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L146'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:146:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
... 162 additional changes omitted for rule AIR302
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L147'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:147:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L208'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:208:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L111'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:111:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L112'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:112:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
... 157 additional changes omitted for rule AIR301
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR303 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
... 317 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 178 | 5 | 173 | 0 | 0 |
| AIR301 | 173 | 173 | 0 | 0 | 0 |
| AIR303 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @Lee-W on 2025-04-02 15:46_

---

_Comment by @Lee-W on 2025-04-02 16:06_

@dhruvmanila  This is ready as well 🙌 

---

_Renamed from "Rename the original AIR302, AIR303 as AIR301 and AIR302" to "[`airflow`] Move `AIR302` to `AIR301` and `AIR303` to `AIR302`" by @dhruvmanila on 2025-04-02 17:25_

---

_Label `rule` added by @dhruvmanila on 2025-04-02 17:25_

---

_Label `preview` added by @dhruvmanila on 2025-04-02 17:25_

---

_Merged by @dhruvmanila on 2025-04-02 17:31_

---

_Closed by @dhruvmanila on 2025-04-02 17:31_

---

_@dhruvmanila reviewed on 2025-04-02 17:31_

(Oops, forgot to approve first but this looks good)

---
