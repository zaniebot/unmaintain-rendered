```yaml
number: 15015
title: "feat(AIR302): extend the following rules"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR302
created_at: 2024-12-16T09:49:25Z
updated_at: 2024-12-18T00:07:17Z
url: https://github.com/astral-sh/ruff/pull/15015
synced_at: 2026-01-12T15:55:49Z
```

# feat(AIR302): extend the following rules

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

Airflow 3.0 removes various deprecated functions, members, modules, and other values. They have been deprecated in 2.x, but the removal causes incompatibilities that we want to detect. This PR deprecates the following names.

* `airflow.api_connexion.security.requires_access_dataset` → `airflow.api_connexion.security.requires_access_asset`
* `airflow.auth.managers.base_auth_manager.is_authorized_dataset` → `airflow.auth.managers.base_auth_manager.is_authorized_asset`
* `airflow.auth.managers.models.resource_details.DatasetDetails` → `airflow.auth.managers.models.resource_details.AssetDetails`
* `airflow.lineage.hook.DatasetLineageInfo` → `airflow.lineage.hook.AssetLineageInfo`
* `airflow.security.permissions.RESOURCE_DATASET` → `airflow.security.permissions.RESOURCE_ASSET`
* `airflow.www.auth.has_access_dataset` → `airflow.www.auth.has_access_dataset.has_access_asset`
* remove `airflow.datasets.DatasetAliasEvent`
* `airflow.datasets.Dataset` → `airflow.sdk.definitions.asset.Asset`
* `airflow.Dataset` → `airflow.sdk.definitions.asset.Asset`
* `airflow.datasets.DatasetAlias` → `airflow.sdk.definitions.asset.AssetAlias`
* `airflow.datasets.DatasetAll` → `airflow.sdk.definitions.asset.AssetAll`
* `airflow.datasets.DatasetAny` → `airflow.sdk.definitions.asset.AssetAny`
* `airflow.datasets.metadata` → `airflow.sdk.definitions.asset.metadata`
* `airflow.datasets.expand_alias_to_datasets` → `airflow.sdk.definitions.asset.expand_alias_to_assets`
* `airflow.datasets.manager.dataset_manager` → `airflow.assets.manager`
* `airflow.datasets.manager.resolve_dataset_manager` → `airflow.assets.resolve_asset_manager`
* `airflow.datasets.manager.DatasetManager` → `airflow.assets.AssetManager`
* `airflow.listeners.spec.dataset.on_dataset_created` → `airflow.listeners.spec.asset.on_asset_created`
* `airflow.listeners.spec.dataset.on_dataset_changed` → `airflow.listeners.spec.asset.on_asset_changed`
* `airflow.timetables.simple.DatasetTriggeredTimetable` → `airflow.timetables.simple.AssetTriggeredTimetable`
* `airflow.timetables.datasets.DatasetOrTimeSchedule` → `airflow.timetables.assets.AssetOrTimeSchedule`
* `airflow.providers.amazon.auth_manager.avp.entities.AvpEntities.DATASET` → `airflow.providers.amazon.auth_manager.avp.entities.AvpEntities.ASSET`
* `airflow.providers.amazon.aws.datasets.s3.create_dataset` → `airflow.providers.amazon.aws.assets.s3.create_asset`
* `airflow.providers.amazon.aws.datasets.s3.convert_dataset_to_openlineage` → `airflow.providers.amazon.aws.datasets.s3.convert_dataset_to_openlineage`
* `airflow.providers.amazon.aws.datasets.s3.sanitize_uri` → `airflow.providers.amazon.aws.assets.s3.sanitize_uri`
* `airflow.providers.common.io.datasets.file.convert_dataset_to_openlineage` → `airflow.providers.common.io.assets.file.convert_asset_to_openlineage`
* `airflow.providers.common.io.datasets.file.sanitize_uri` → `airflow.providers.common.io.assets.file.sanitize_uri`
* `airflow.providers.common.io.datasets.file.create_dataset` → `airflow.providers.common.io.assets.file.create_asset`
* `airflow.providers.google.datasets.bigquery.sanitize_uri` → `airflow.providers.google.assets.bigquery.sanitize_uri`
* `airflow.providers.google.datasets.gcs.create_dataset` → `airflow.providers.google.assets.gcs.create_asset`
* `airflow.providers.google.datasets.gcs.sanitize_uri` → `airflow.providers.google.assets.gcs.sanitize_uri`
* `airflow.providers.google.datasets.gcs.convert_dataset_to_openlineage` → `airflow.providers.google.assets.gcs.convert_asset_to_openlineage`
* `airflow.providers.fab.auth_manager.fab_auth_manager.is_authorized_dataset` → `airflow.providers.fab.auth_manager.fab_auth_manager.is_authorized_asset`
* `airflow.providers.openlineage.utils.utils.DatasetInfo` → `airflow.providers.openlineage.utils.utils.AssetInfo`
* `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` → `airflow.providers.openlineage.utils.utils.translate_airflow_asset`
* `airflow.providers.postgres.datasets.postgres.sanitize_uri` → `airflow.providers.postgres.assets.postgres.sanitize_uri`
* `airflow.providers.mysql.datasets.mysql.sanitize_uri` → `airflow.providers.mysql.assets.mysql.sanitize_uri`
* `airflow.providers.trino.datasets.trino.sanitize_uri` → `airflow.providers.trino.assets.trino.sanitize_uri`

In additional to the newly added rules above, the message for `airflow.contrib.*` and `airflow.subdag.*` has been extended, `airflow.sensors.external_task.ExternalTaskSensorLink` error has been fixed and the test fixture has been reorganized

## Test Plan

<!-- How was it tested? -->
A test fixture is included in the PR.


---

_Comment by @github-actions[bot] on 2024-12-16 09:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+16 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/src/airflow/providers/common/compat/lineage/hook.py:39:32:</a> AIR302 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/compat/lineage/hook.py#L39'>providers/src/airflow/providers/common/compat/lineage/hook.py:39:5:</a> AIR302 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/compat/lineage/hook.py#L63'>providers/src/airflow/providers/common/compat/lineage/hook.py:63:17:</a> AIR302 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/compat/lineage/hook.py#L67'>providers/src/airflow/providers/common/compat/lineage/hook.py:67:17:</a> AIR302 `airflow.lineage.hook.DatasetLineageInfo` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/compat/openlineage/utils/utils.py#L40'>providers/src/airflow/providers/common/compat/openlineage/utils/utils.py:40:59:</a> AIR302 `airflow.providers.openlineage.utils.utils.translate_airflow_dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/io/assets/file.py#L35'>providers/src/airflow/providers/common/io/assets/file.py:35:47:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/io/assets/file.py#L37'>providers/src/airflow/providers/common/io/assets/file.py:37:12:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/common/io/assets/file.py#L46'>providers/src/airflow/providers/common/io/assets/file.py:46:41:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/google/assets/gcs.py#L40'>providers/src/airflow/providers/google/assets/gcs.py:40:74:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/google/assets/gcs.py#L41'>providers/src/airflow/providers/google/assets/gcs.py:41:12:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/google/assets/gcs.py#L50'>providers/src/airflow/providers/google/assets/gcs.py:50:41:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/openlineage/utils/utils.py#L402'>providers/src/airflow/providers/openlineage/utils/utils.py:402:84:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/openlineage/utils/utils.py#L403'>providers/src/airflow/providers/openlineage/utils/utils.py:403:86:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/openlineage/utils/utils.py#L723'>providers/src/airflow/providers/openlineage/utils/utils.py:723:36:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/src/airflow/providers/openlineage/utils/utils.py#L758'>providers/src/airflow/providers/openlineage/utils/utils.py:758:36:</a> AIR302 `airflow.datasets.Dataset` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/bccd7202266b75f610d97c947a6da684592c3b5d/providers/tests/openlineage/plugins/test_utils.py#L406'>providers/tests/openlineage/plugins/test_utils.py:406:21:</a> AIR302 `airflow.timetables.simple.DatasetTriggeredTimetable` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 16 | 16 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @dhruvmanila on 2024-12-16 10:24_

---

_Label `preview` added by @dhruvmanila on 2024-12-16 10:24_

---

_Marked ready for review by @Lee-W on 2024-12-17 04:20_

---

_Merged by @MichaReiser on 2024-12-17 07:32_

---

_Closed by @MichaReiser on 2024-12-17 07:32_

---

_Branch deleted on 2024-12-18 00:07_

---
