```yaml
number: 20082
title: "[`airflow`] Extend `AIR311` and `AIR312` rules"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR-31x-rules
created_at: 2025-08-25T14:39:06Z
updated_at: 2025-08-27T18:11:22Z
url: https://github.com/astral-sh/ruff/pull/20082
synced_at: 2026-01-12T15:56:54Z
```

# [`airflow`] Extend `AIR311` and `AIR312` rules

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

Extend the following rules.

### AIR311
* `airflow.sensors.base.BaseSensorOperator` → airflow.sdk.bases.sensor.BaseSensorOperator`
* `airflow.sensors.base.PokeReturnValue` → airflow.sdk.bases.sensor.PokeReturnValue`
* `airflow.sensors.base.poke_mode_only` → airflow.sdk.bases.sensor.poke_mode_only`
* `airflow.decorators.base.DecoratedOperator` → airflow.sdk.bases.decorator.DecoratedOperator`
* `airflow.models.param.Param` → airflow.sdk.definitions.param.Param`
* `airflow.decorators.base.DecoratedMappedOperator` → `airflow.sdk.bases.decorator.DecoratedMappedOperator`
* `airflow.decorators.base.DecoratedOperator` → `airflow.sdk.bases.decorator.DecoratedOperator`
* `airflow.decorators.base.TaskDecorator` → `airflow.sdk.bases.decorator.TaskDecorator`
* `airflow.decorators.base.get_unique_task_id` → `airflow.sdk.bases.decorator.get_unique_task_id`
* `airflow.decorators.base.task_decorator_factory` → `airflow.sdk.bases.decorator.task_decorator_factory`


### AIR312
* `airflow.sensors.bash.BashSensor` → `airflow.providers.standard.sensor.bash.BashSensor`
* `airflow.sensors.python.PythonSensor` → `airflow.providers.standard.sensors.python.PythonSensor`



## Test Plan

<!-- How was it tested? -->

update the test fixture accordingly in the second commit and reorg in the third


---

_Comment by @github-actions[bot] on 2025-08-25 14:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+88 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+88 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/amazon/src/airflow/providers/amazon/aws/sensors/s3.py#L223'>providers/amazon/src/airflow/providers/amazon/aws/sensors/s3.py:223:2:</a> AIR311 `airflow.sensors.base.poke_mode_only` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/celery/src/airflow/providers/celery/sensors/celery_queue.py#L39'>providers/celery/src/airflow/providers/celery/sensors/celery_queue.py:39:25:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/common/sql/src/airflow/providers/common/sql/sensors/sql.pyi#L49'>providers/common/sql/src/airflow/providers/common/sql/sensors/sql.pyi:49:17:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/databricks/src/airflow/providers/databricks/sensors/databricks.py#L43'>providers/databricks/src/airflow/providers/databricks/sensors/databricks.py:43:67:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/databricks/src/airflow/providers/databricks/sensors/databricks_partition.py#L48'>providers/databricks/src/airflow/providers/databricks/sensors/databricks_partition.py:48:33:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/databricks/src/airflow/providers/databricks/sensors/databricks_sql.py#L45'>providers/databricks/src/airflow/providers/databricks/sensors/databricks_sql.py:45:27:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/dbt/cloud/src/airflow/providers/dbt/cloud/sensors/dbt.py#L40'>providers/dbt/cloud/src/airflow/providers/dbt/cloud/sensors/dbt.py:40:28:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/docker/src/airflow/providers/docker/decorators/docker.py#L102'>providers/docker/src/airflow/providers/docker/decorators/docker.py:102:32:</a> AIR311 `airflow.decorators.base.DecoratedOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/docker/src/airflow/providers/docker/decorators/docker.py#L215'>providers/docker/src/airflow/providers/docker/decorators/docker.py:215:12:</a> AIR311 `airflow.decorators.base.task_decorator_factory` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/bigquery_dts.py#L44'>providers/google/src/airflow/providers/google/cloud/sensors/bigquery_dts.py:44:52:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/bigtable.py#L44'>providers/google/src/airflow/providers/google/cloud/sensors/bigtable.py:44:47:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/cloud_composer.py#L48'>providers/google/src/airflow/providers/google/cloud/sensors/cloud_composer.py:48:33:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/cloud_storage_transfer_service.py#L49'>providers/google/src/airflow/providers/google/cloud/sensors/cloud_storage_transfer_service.py:49:47:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataform.py#L114'>providers/google/src/airflow/providers/google/cloud/sensors/dataform.py:114:51:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataform.py#L38'>providers/google/src/airflow/providers/google/cloud/sensors/dataform.py:38:45:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/datafusion.py#L39'>providers/google/src/airflow/providers/google/cloud/sensors/datafusion.py:39:42:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataplex.py#L135'>providers/google/src/airflow/providers/google/cloud/sensors/dataplex.py:135:42:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataplex.py#L253'>providers/google/src/airflow/providers/google/cloud/sensors/dataplex.py:253:42:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataplex.py#L58'>providers/google/src/airflow/providers/google/cloud/sensors/dataplex.py:58:31:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataprep.py#L37'>providers/google/src/airflow/providers/google/cloud/sensors/dataprep.py:37:40:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataproc.py#L125'>providers/google/src/airflow/providers/google/cloud/sensors/dataproc.py:125:27:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataproc.py#L43'>providers/google/src/airflow/providers/google/cloud/sensors/dataproc.py:43:25:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/dataproc_metastore.py#L39'>providers/google/src/airflow/providers/google/cloud/sensors/dataproc_metastore.py:39:36:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/gcs.py#L161'>providers/google/src/airflow/providers/google/cloud/sensors/gcs.py:161:29:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/gcs.py#L252'>providers/google/src/airflow/providers/google/cloud/sensors/gcs.py:252:43:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/gcs.py#L345'>providers/google/src/airflow/providers/google/cloud/sensors/gcs.py:345:2:</a> AIR311 `airflow.sensors.base.poke_mode_only` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/gcs.py#L346'>providers/google/src/airflow/providers/google/cloud/sensors/gcs.py:346:38:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/gcs.py#L53'>providers/google/src/airflow/providers/google/cloud/sensors/gcs.py:53:32:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/looker.py#L37'>providers/google/src/airflow/providers/google/cloud/sensors/looker.py:37:33:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/pubsub.py#L48'>providers/google/src/airflow/providers/google/cloud/sensors/pubsub.py:48:24:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/tasks.py#L38'>providers/google/src/airflow/providers/google/cloud/sensors/tasks.py:38:28:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/vertex_ai/feature_store.py#L39'>providers/google/src/airflow/providers/google/cloud/sensors/vertex_ai/feature_store.py:39:29:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/cloud/sensors/workflows.py#L41'>providers/google/src/airflow/providers/google/cloud/sensors/workflows.py:41:31:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/marketing_platform/sensors/campaign_manager.py#L37'>providers/google/src/airflow/providers/google/marketing_platform/sensors/campaign_manager.py:37:41:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/marketing_platform/sensors/display_video.py#L105'>providers/google/src/airflow/providers/google/marketing_platform/sensors/display_video.py:105:43:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/marketing_platform/sensors/display_video.py#L38'>providers/google/src/airflow/providers/google/marketing_platform/sensors/display_video.py:38:58:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/google/src/airflow/providers/google/suite/sensors/drive.py#L37'>providers/google/src/airflow/providers/google/suite/sensors/drive.py:37:38:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/http/src/airflow/providers/http/sensors/http.py#L110'>providers/http/src/airflow/providers/http/sensors/http.py:110:46:</a> AIR311 `airflow.sensors.base.PokeReturnValue` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/http/src/airflow/providers/http/sensors/http.py#L137'>providers/http/src/airflow/providers/http/sensors/http.py:137:48:</a> AIR311 `airflow.sensors.base.PokeReturnValue` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/http/tests/unit/http/sensors/test_http.py#L82'>providers/http/tests/unit/http/sensors/test_http.py:82:20:</a> AIR311 `airflow.sensors.base.PokeReturnValue` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/cosmos.py#L35'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/cosmos.py:35:33:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/data_factory.py#L43'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/data_factory.py:43:47:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/msgraph.py#L44'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/msgraph.py:44:21:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/wasb.py#L117'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/wasb.py:117:24:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/wasb.py#L39'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/sensors/wasb.py:39:22:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/openlineage/src/airflow/providers/openlineage/utils/selective_enable.py#L38'>providers/openlineage/src/airflow/providers/openlineage/utils/selective_enable.py:38:19:</a> AIR311 `airflow.models.Param` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/openlineage/src/airflow/providers/openlineage/utils/selective_enable.py#L39'>providers/openlineage/src/airflow/providers/openlineage/utils/selective_enable.py:39:20:</a> AIR311 `airflow.models.Param` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L345'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:345:36:</a> AIR311 `airflow.sensors.base.BaseSensorOperator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/sftp/src/airflow/providers/sftp/decorators/sensors/sftp.py#L65'>providers/sftp/src/airflow/providers/sftp/decorators/sensors/sftp.py:65:29:</a> AIR311 `airflow.decorators.base.get_unique_task_id` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/700b43c0731c8c6c90604c1c3066fd30004b50dd/providers/sftp/src/airflow/providers/sftp/decorators/sensors/sftp.py#L69'>providers/sftp/src/airflow/providers/sftp/decorators/sensors/sftp.py:69:76:</a> AIR311 `airflow.decorators.base.TaskDecorator` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 38 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 88 | 88 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "feat(AIR31X): extend AIR31x rules" to "[`airflow`]: extend AIR311 and AIR312 rules" by @Lee-W on 2025-08-27 07:36_

---

_Marked ready for review by @Lee-W on 2025-08-27 09:23_

---

_Label `rule` added by @ntBre on 2025-08-27 17:46_

---

_Label `preview` added by @ntBre on 2025-08-27 17:46_

---

_@ntBre approved on 2025-08-27 18:10_

Thanks!

---

_Renamed from "[`airflow`]: extend AIR311 and AIR312 rules" to "[`airflow`] Extend `AIR311` and `AIR312` rules" by @ntBre on 2025-08-27 18:11_

---

_Merged by @ntBre on 2025-08-27 18:11_

---

_Closed by @ntBre on 2025-08-27 18:11_

---
