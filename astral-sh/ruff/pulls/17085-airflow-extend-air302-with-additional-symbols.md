```yaml
number: 17085
title: "[`airflow`] Extend `AIR302` with additional symbols"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-new-AIR302-rules
created_at: 2025-03-31T09:14:11Z
updated_at: 2025-04-02T15:08:52Z
url: https://github.com/astral-sh/ruff/pull/17085
synced_at: 2026-01-12T15:56:00Z
```

# [`airflow`] Extend `AIR302` with additional symbols

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

* ``airflow.auth.managers.base_auth_manager.is_authorized_dataset`` has been moved to ``airflow.api_fastapi.auth.managers.base_auth_manager.is_authorized_asset`` in Airflow 3.0
* ``airflow.auth.managers.models.resource_details.DatasetDetails`` has been moved to ``airflow.api_fastapi.auth.managers.models.resource_details.AssetDetails`` in Airflow 3.0
* Dag arguments `default_view` and `orientation` has been removed in Airflow 3.0
* `airflow.models.baseoperatorlink.BaseOperatorLink` has been moved to `airflow.sdk.definitions.baseoperatorlink.BaseOperatorLink` in Airflow 3.0
* ``airflow.notifications.basenotifier.BaseNotifier`` has been moved to ``airflow.sdk.BaseNotifier``  in Airflow 3.0
* ``airflow.utils.log.secrets_masker`` has been moved to ``airflow.sdk.execution_time.secrets_masker`` in Airflow 3.0
* ``airflow...DAG.allow_future_exec_dates`` has been removed in Airflow 3.0
* `airflow.utils.db.create_session` has een removed in Airflow 3.0
* `airflow.sensors.base_sensor_operator.BaseSensorOperator` has been moved to `airflow.sdk.bases.sensor.BaseSensorOperator` removed Airflow 3.0
* `airflow.utils.file.TemporaryDirectory` has been removed in Airflow 3.0 and can be replaced by `tempfile.TemporaryDirectory`
*  `airflow.utils.file.mkdirs` has been removed in Airflow 3.0 and can be replaced by `pathlib.Path({path}).mkdir`

## Test Plan

<!-- How was it tested? -->
Test fixture has been added for these changes

---

_Comment by @github-actions[bot] on 2025-03-31 09:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+21 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/dev/airflow_perf/scheduler_dag_execution_timing.py#L256'>dev/airflow_perf/scheduler_dag_execution_timing.py:256:13:</a> AIR302 `airflow.utils.db.create_session` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/dev/airflow_perf/scheduler_dag_execution_timing.py#L303'>dev/airflow_perf/scheduler_dag_execution_timing.py:303:21:</a> AIR302 `airflow.utils.db.create_session` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py#L41'>providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py:41:19:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/databricks/src/airflow/providers/databricks/operators/databricks.py#L242'>providers/databricks/src/airflow/providers/databricks/operators/databricks.py:242:28:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py#L234'>providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py:234:26:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py#L268'>providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py:268:38:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py#L362'>providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py:362:39:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/dbt/cloud/src/airflow/providers/dbt/cloud/operators/dbt.py#L50'>providers/dbt/cloud/src/airflow/providers/dbt/cloud/operators/dbt.py:50:34:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/cloud/links/base.py#L38'>providers/google/src/airflow/providers/google/cloud/links/base.py:38:22:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/cloud/links/datafusion.py#L44'>providers/google/src/airflow/providers/google/cloud/links/datafusion.py:44:22:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/cloud/links/dataproc.py#L136'>providers/google/src/airflow/providers/google/cloud/links/dataproc.py:136:24:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/cloud/links/dataproc.py#L83'>providers/google/src/airflow/providers/google/cloud/links/dataproc.py:83:20:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L112'>providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:112:37:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L64'>providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:64:29:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/google/src/airflow/providers/google/marketing_platform/links/analytics_admin.py#L39'>providers/google/src/airflow/providers/google/marketing_platform/links/analytics_admin.py:39:31:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/data_factory.py#L52'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/data_factory.py:52:53:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/powerbi.py#L42'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/powerbi.py:42:19:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/synapse.py#L133'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/synapse.py:133:35:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/standard/src/airflow/providers/standard/operators/trigger_dagrun.py#L71'>providers/standard/src/airflow/providers/standard/operators/trigger_dagrun.py:71:25:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/standard/src/airflow/providers/standard/sensors/external_task.py#L58'>providers/standard/src/airflow/providers/standard/sensors/external_task.py:58:23:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/d4473555c0e7022e073489b7163d49102881a1a6/providers/yandex/src/airflow/providers/yandex/links/yq.py#L43'>providers/yandex/src/airflow/providers/yandex/links/yq.py:43:14:</a> AIR302 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 21 | 21 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "feat(AIR302): add rule "airflow.models.baseoperatorlink.BaseOperatorL…" to "[airflow] extend AIR302 rules" by @Lee-W on 2025-03-31 10:50_

---

_Marked ready for review by @Lee-W on 2025-03-31 15:05_

---

_Label `rule` added by @dhruvmanila on 2025-03-31 21:06_

---

_Label `preview` added by @dhruvmanila on 2025-03-31 21:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:745 on 2025-03-31 21:10_

Do we need to add the `db` module in here? i.e., `["db", "create_session"]` based on the comment because we only match against `airflow.utils`, the remaining is matched here.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_names.py`:297 on 2025-03-31 21:15_

This is not being considered (refer to my other comment) as seen in the snapshot.

Can you make sure to verify the snapshot changes? I know they're a bit hard to read when the changes are in the middle of the file. It might be easier to add new test cases at the end of the file to make snapshot changes easier to read and then moving it back to the relevant place and update the "Test plan" in the PR description to mention if you've done this.

---

_@dhruvmanila requested changes on 2025-03-31 21:16_

---

_@Lee-W reviewed on 2025-04-01 01:09_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:745 on 2025-04-01 01:09_

Yes, just fixed. Thanks!

---

_@Lee-W reviewed on 2025-04-01 01:57_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_names.py`:297 on 2025-04-01 01:57_

> This is not being considered (refer to my other comment) as seen in the snapshot.
Hi, I apologize for missing this one.

> It might be easier to add new test cases at the end of the file to make snapshot changes easier to read and then moving it back
This is an Great idea. I used to perform text searches on the git diff, but it's easy to overlook. I'll take care of it afterward. Thank you so much!

---

_Review requested from @dhruvmanila by @Lee-W on 2025-04-01 02:00_

---

_@dhruvmanila approved on 2025-04-02 15:08_

---

_Renamed from "[airflow] extend AIR302 rules" to "[`airflow`] Extend `AIR302` with additional symbols" by @dhruvmanila on 2025-04-02 15:08_

---

_Merged by @dhruvmanila on 2025-04-02 15:08_

---

_Closed by @dhruvmanila on 2025-04-02 15:08_

---
