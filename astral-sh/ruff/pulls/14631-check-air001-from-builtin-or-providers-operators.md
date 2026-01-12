```yaml
number: 14631
title: "Check `AIR001` from builtin or providers `operators` module"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/air001
created_at: 2024-11-27T10:20:20Z
updated_at: 2024-12-04T17:23:47Z
url: https://github.com/astral-sh/ruff/pull/14631
synced_at: 2026-01-12T15:55:48Z
```

# Check `AIR001` from builtin or providers `operators` module

---

_@dhruvmanila_

## Summary

This PR makes changes to the `AIR001` rule as per https://github.com/astral-sh/ruff/pull/14627#discussion_r1860212307.

Additionally,
* Avoid returning the `Diagnostic` and update the checker in the rule logic for consistency
* Remove test case for different keyword position (I don't think it's required here)

## Test Plan

Add test cases for multiple operators from various modules.


---

_Comment by @github-actions[bot] on 2024-11-27 10:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -678 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -678 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L64'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:64:5:</a> AIR001 Task variable name should match the `task_id`: "sum_it"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L102'>airflow/example_dags/example_sensors.py:102:5:</a> AIR001 Task variable name should match the `task_id`: "wait_for_file_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L112'>airflow/example_dags/example_sensors.py:112:5:</a> AIR001 Task variable name should match the `task_id`: "success_sensor_python"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L114'>airflow/example_dags/example_sensors.py:114:5:</a> AIR001 Task variable name should match the `task_id`: "failure_timeout_sensor_python"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L120'>airflow/example_dags/example_sensors.py:120:5:</a> AIR001 Task variable name should match the `task_id`: "week_day_sensor_failing_on_timeout"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L56'>airflow/example_dags/example_sensors.py:56:5:</a> AIR001 Task variable name should match the `task_id`: "wait_some_seconds"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L60'>airflow/example_dags/example_sensors.py:60:5:</a> AIR001 Task variable name should match the `task_id`: "wait_some_seconds_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L64'>airflow/example_dags/example_sensors.py:64:5:</a> AIR001 Task variable name should match the `task_id`: "fire_immediately"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L68'>airflow/example_dags/example_sensors.py:68:5:</a> AIR001 Task variable name should match the `task_id`: "timeout_after_second_date_in_the_future"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L77'>airflow/example_dags/example_sensors.py:77:5:</a> AIR001 Task variable name should match the `task_id`: "fire_immediately_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L81'>airflow/example_dags/example_sensors.py:81:5:</a> AIR001 Task variable name should match the `task_id`: "timeout_after_second_date_in_the_future_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L90'>airflow/example_dags/example_sensors.py:90:5:</a> AIR001 Task variable name should match the `task_id`: "Sensor_succeeds"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L92'>airflow/example_dags/example_sensors.py:92:5:</a> AIR001 Task variable name should match the `task_id`: "Sensor_fails_after_3_seconds"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L98'>airflow/example_dags/example_sensors.py:98:5:</a> AIR001 Task variable name should match the `task_id`: "wait_for_file"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L34'>providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py:34:1:</a> AIR001 Task variable name should match the `task_id`: "aql_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L46'>providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py:46:1:</a> AIR001 Task variable name should match the `task_id`: "aql_sensor_template_file"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_cloud_task.py#L48'>providers/src/airflow/providers/google/cloud/example_dags/example_cloud_task.py:48:5:</a> AIR001 Task variable name should match the `task_id`: "gcp_sense_cloud_tasks_empty"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L102'>providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:102:5:</a> AIR001 Task variable name should match the `task_id`: "gcs_to_bq_example"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L91'>providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:91:5:</a> AIR001 Task variable name should match the `task_id`: "run_fetch_data"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py#L60'>providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py:60:5:</a> AIR001 Task variable name should match the `task_id`: "upload_to_gcs"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py#L95'>providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py:95:5:</a> AIR001 Task variable name should match the `task_id`: "gcs_to_bq"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_batch.py#L111'>providers/tests/amazon/aws/sensors/test_batch.py:111:9:</a> AIR001 Task variable name should match the `task_id`: "batch_job_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L108'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:108:9:</a> AIR001 Task variable name should match the `task_id`: "cf_delete_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L119'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:119:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L124'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:124:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L129'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:129:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L135'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:135:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L44'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:44:9:</a> AIR001 Task variable name should match the `task_id`: "cf_create_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L61'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:61:9:</a> AIR001 Task variable name should match the `task_id`: "cf_create_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L70'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:70:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L75'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:75:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L80'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:80:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L91'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:91:9:</a> AIR001 Task variable name should match the `task_id`: "cf_delete_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_dms.py#L61'>providers/tests/amazon/aws/sensors/test_dms.py:61:9:</a> AIR001 Task variable name should match the `task_id`: "create_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_dynamodb.py#L155'>providers/tests/amazon/aws/sensors/test_dynamodb.py:155:9:</a> AIR001 Task variable name should match the `task_id`: "dynamodb_value_sensor_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_dynamodb.py#L178'>providers/tests/amazon/aws/sensors/test_dynamodb.py:178:9:</a> AIR001 Task variable name should match the `task_id`: "dynamodb_value_sensor_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_ec2.py#L30'>providers/tests/amazon/aws/sensors/test_ec2.py:30:9:</a> AIR001 Task variable name should match the `task_id`: "task_test"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_ecs.py#L84'>providers/tests/amazon/aws/sensors/test_ecs.py:84:9:</a> AIR001 Task variable name should match the `task_id`: "test_ecs_base"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_ecs.py#L95'>providers/tests/amazon/aws/sensors/test_ecs.py:95:9:</a> AIR001 Task variable name should match the `task_id`: "test_ecs_base_hook_client"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L207'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:207:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L226'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:226:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L248'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:248:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L265'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:265:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py#L44'>providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py:44:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py#L56'>providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py:56:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py#L70'>providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py:70:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_step.py#L205'>providers/tests/amazon/aws/sensors/test_emr_step.py:205:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_glue.py#L120'>providers/tests/amazon/aws/sensors/test_glue.py:120:9:</a> AIR001 Task variable name should match the `task_id`: "test_glue_job_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_glue.py#L141'>providers/tests/amazon/aws/sensors/test_glue.py:141:9:</a> AIR001 Task variable name should match the `task_id`: "test_glue_job_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_glue.py#L36'>providers/tests/amazon/aws/sensors/test_glue.py:36:9:</a> AIR001 Task variable name should match the `task_id`: "test_glue_job_sensor"
... 628 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR001 | 678 | 0 | 678 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -678 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -678 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L64'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:64:5:</a> AIR001 Task variable name should match the `task_id`: "sum_it"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L102'>airflow/example_dags/example_sensors.py:102:5:</a> AIR001 Task variable name should match the `task_id`: "wait_for_file_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L112'>airflow/example_dags/example_sensors.py:112:5:</a> AIR001 Task variable name should match the `task_id`: "success_sensor_python"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L114'>airflow/example_dags/example_sensors.py:114:5:</a> AIR001 Task variable name should match the `task_id`: "failure_timeout_sensor_python"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L120'>airflow/example_dags/example_sensors.py:120:5:</a> AIR001 Task variable name should match the `task_id`: "week_day_sensor_failing_on_timeout"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L56'>airflow/example_dags/example_sensors.py:56:5:</a> AIR001 Task variable name should match the `task_id`: "wait_some_seconds"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L60'>airflow/example_dags/example_sensors.py:60:5:</a> AIR001 Task variable name should match the `task_id`: "wait_some_seconds_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L64'>airflow/example_dags/example_sensors.py:64:5:</a> AIR001 Task variable name should match the `task_id`: "fire_immediately"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L68'>airflow/example_dags/example_sensors.py:68:5:</a> AIR001 Task variable name should match the `task_id`: "timeout_after_second_date_in_the_future"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L77'>airflow/example_dags/example_sensors.py:77:5:</a> AIR001 Task variable name should match the `task_id`: "fire_immediately_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L81'>airflow/example_dags/example_sensors.py:81:5:</a> AIR001 Task variable name should match the `task_id`: "timeout_after_second_date_in_the_future_async"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L90'>airflow/example_dags/example_sensors.py:90:5:</a> AIR001 Task variable name should match the `task_id`: "Sensor_succeeds"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L92'>airflow/example_dags/example_sensors.py:92:5:</a> AIR001 Task variable name should match the `task_id`: "Sensor_fails_after_3_seconds"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/airflow/example_dags/example_sensors.py#L98'>airflow/example_dags/example_sensors.py:98:5:</a> AIR001 Task variable name should match the `task_id`: "wait_for_file"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L34'>providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py:34:1:</a> AIR001 Task variable name should match the `task_id`: "aql_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L46'>providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py:46:1:</a> AIR001 Task variable name should match the `task_id`: "aql_sensor_template_file"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_cloud_task.py#L48'>providers/src/airflow/providers/google/cloud/example_dags/example_cloud_task.py:48:5:</a> AIR001 Task variable name should match the `task_id`: "gcp_sense_cloud_tasks_empty"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L102'>providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:102:5:</a> AIR001 Task variable name should match the `task_id`: "gcs_to_bq_example"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L91'>providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:91:5:</a> AIR001 Task variable name should match the `task_id`: "run_fetch_data"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py#L60'>providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py:60:5:</a> AIR001 Task variable name should match the `task_id`: "upload_to_gcs"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py#L95'>providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py:95:5:</a> AIR001 Task variable name should match the `task_id`: "gcs_to_bq"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_batch.py#L111'>providers/tests/amazon/aws/sensors/test_batch.py:111:9:</a> AIR001 Task variable name should match the `task_id`: "batch_job_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L108'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:108:9:</a> AIR001 Task variable name should match the `task_id`: "cf_delete_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L119'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:119:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L124'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:124:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L129'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:129:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L135'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:135:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L44'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:44:9:</a> AIR001 Task variable name should match the `task_id`: "cf_create_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L61'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:61:9:</a> AIR001 Task variable name should match the `task_id`: "cf_create_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L70'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:70:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L75'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:75:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L80'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:80:9:</a> AIR001 Task variable name should match the `task_id`: "task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_cloud_formation.py#L91'>providers/tests/amazon/aws/sensors/test_cloud_formation.py:91:9:</a> AIR001 Task variable name should match the `task_id`: "cf_delete_stack_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_dms.py#L61'>providers/tests/amazon/aws/sensors/test_dms.py:61:9:</a> AIR001 Task variable name should match the `task_id`: "create_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_dynamodb.py#L155'>providers/tests/amazon/aws/sensors/test_dynamodb.py:155:9:</a> AIR001 Task variable name should match the `task_id`: "dynamodb_value_sensor_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_dynamodb.py#L178'>providers/tests/amazon/aws/sensors/test_dynamodb.py:178:9:</a> AIR001 Task variable name should match the `task_id`: "dynamodb_value_sensor_init"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_ec2.py#L30'>providers/tests/amazon/aws/sensors/test_ec2.py:30:9:</a> AIR001 Task variable name should match the `task_id`: "task_test"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_ecs.py#L84'>providers/tests/amazon/aws/sensors/test_ecs.py:84:9:</a> AIR001 Task variable name should match the `task_id`: "test_ecs_base"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_ecs.py#L95'>providers/tests/amazon/aws/sensors/test_ecs.py:95:9:</a> AIR001 Task variable name should match the `task_id`: "test_ecs_base_hook_client"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L207'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:207:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L226'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:226:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L248'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:248:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_job_flow.py#L265'>providers/tests/amazon/aws/sensors/test_emr_job_flow.py:265:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py#L44'>providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py:44:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py#L56'>providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py:56:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py#L70'>providers/tests/amazon/aws/sensors/test_emr_notebook_execution.py:70:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_emr_step.py#L205'>providers/tests/amazon/aws/sensors/test_emr_step.py:205:9:</a> AIR001 Task variable name should match the `task_id`: "test_task"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_glue.py#L120'>providers/tests/amazon/aws/sensors/test_glue.py:120:9:</a> AIR001 Task variable name should match the `task_id`: "test_glue_job_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_glue.py#L141'>providers/tests/amazon/aws/sensors/test_glue.py:141:9:</a> AIR001 Task variable name should match the `task_id`: "test_glue_job_sensor"
- <a href='https://github.com/apache/airflow/blob/93bcfc2bfeecb4c044c1570053959427e52eee20/providers/tests/amazon/aws/sensors/test_glue.py#L36'>providers/tests/amazon/aws/sensors/test_glue.py:36:9:</a> AIR001 Task variable name should match the `task_id`: "test_glue_job_sensor"
... 628 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR001 | 678 | 0 | 678 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-11-27 10:28_

CC: @uranusjr 

---

_Comment by @MichaReiser on 2024-11-27 10:29_

That's a lot of removed diagnostics

---

_Label `rule` added by @MichaReiser on 2024-11-27 10:29_

---

_Comment by @dhruvmanila on 2024-11-27 10:31_

> That's a lot of removed diagnostics

~That's because I've removed a couple of test cases which just moves the keyword argument through different position. I don't think the rule should test that.~

Oops, you meant the ecosystem changes and not the snapshot changes.

---

_@dhruvmanila reviewed on 2024-11-27 10:31_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/airflow/AIR001.py`:9 on 2024-11-27 10:31_

cc @MichaReiser these two test cases are identical except that the arguments are reversed.

---

_Comment by @MichaReiser on 2024-11-27 10:36_

Yeah. I'd like for @uranusjr to have a look at the ecosystem changes. I worry that we just removed many true-positives, because Ruff can't see through other import/classes.

---

_Comment by @MichaReiser on 2024-12-03 08:34_

@uranusjr sorry for pinging you again. Would you mind taking a look at the ecosystem changes? I want to make sure that we don't regress an airflow rule.

---

_@MichaReiser approved on 2024-12-04 07:55_

---

_Comment by @MichaReiser on 2024-12-04 07:55_

See https://github.com/astral-sh/ruff/issues/14626#issuecomment-2516358827

---

_Merged by @dhruvmanila on 2024-12-04 08:00_

---

_Closed by @dhruvmanila on 2024-12-04 08:00_

---

_Branch deleted on 2024-12-04 08:00_

---

_Comment by @uranusjr on 2024-12-04 11:57_

I’m late, but this looks good to me as well. Awesome work.

---

_Comment by @dhruvmanila on 2024-12-04 17:23_

> I’m late, but this looks good to me as well. Awesome work.

No worries, thanks for looking into it.

---
