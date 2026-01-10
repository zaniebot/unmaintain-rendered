```yaml
number: 17422
title: "[`airflow`] Extend `AIR311` rules"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-AIR311-rules
created_at: 2025-04-16T08:12:15Z
updated_at: 2025-04-16T16:40:15Z
url: https://github.com/astral-sh/ruff/pull/17422
synced_at: 2026-01-10T19:33:02Z
```

# [`airflow`] Extend `AIR311` rules

---

_Pull request opened by @Lee-W on 2025-04-16 08:12_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

* Extend the following AIR311 rules
    * `airflow.io.path.ObjectStoragePath` → `airflow.sdk.ObjectStoragePath`
    * `airflow.io.storage.attach` → `airflow.sdk.io.attach`
    * `airflow.models.dag.DAG` → `airflow.sdk.DAG`
    * `airflow.models.DAG` → `airflow.sdk.DAG`
    * `airflow.decorators.dag` → `airflow.sdk.dag`
    * `airflow.decorators.task` → `airflow.sdk.task`
    * `airflow.decorators.task_group` → `airflow.sdk.task_group`
    * `airflow.decorators.setup` → `airflow.sdk.setup`
    * `airflow.decorators.teardown` → `airflow.sdk.teardown`

## Test Plan

<!-- How was it tested? -->

The test case has been added to the button of the existing test fixtures, confirmed to be correct and later reorgnaized

---

_Comment by @github-actions[bot] on 2025-04-16 08:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1827 -134 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1827 -134 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L106'>airflow-core/src/airflow/api/common/mark_tasks.py:106:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L142'>airflow-core/src/airflow/api/common/mark_tasks.py:142:22:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L195'>airflow-core/src/airflow/api/common/mark_tasks.py:195:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L239'>airflow-core/src/airflow/api/common/mark_tasks.py:239:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L325'>airflow-core/src/airflow/api/common/mark_tasks.py:325:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L356'>airflow-core/src/airflow/api/common/mark_tasks.py:356:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api/common/mark_tasks.py#L87'>airflow-core/src/airflow/api/common/mark_tasks.py:87:32:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/datamodels/dag_run.py#L108'>airflow-core/src/airflow/api_fastapi/core_api/datamodels/dag_run.py:108:37:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/assets.py#L320'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/assets.py:320:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L159'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:159:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L263'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:263:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L337'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:337:14:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L401'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:401:14:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_versions.py#L109'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_versions.py:109:14:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py#L170'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py:170:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py#L197'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py:197:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/extra_links.py#L58'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/extra_links.py:58:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/task_instances.py#L729'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/task_instances.py:729:12:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/tasks.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/tasks.py:52:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/tasks.py#L77'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/tasks.py:77:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/api_fastapi/core_api/routes/public/xcom.py#L188'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/xcom.py:188:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/cli/commands/dag_command.py#L287'>airflow-core/src/airflow/cli/commands/dag_command.py:287:34:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/cli/commands/dag_command.py#L426'>airflow-core/src/airflow/cli/commands/dag_command.py:426:29:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/cli/commands/dag_command.py#L434'>airflow-core/src/airflow/cli/commands/dag_command.py:434:42:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/cli/commands/dag_command.py#L434'>airflow-core/src/airflow/cli/commands/dag_command.py:434:88:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/airflow-core/src/airflow/cli/commands/dag_command.py#L545'>airflow-core/src/airflow/cli/commands/dag_command.py:545:30:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 1802 additional changes omitted for rule AIR311
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_appflow.py#L102'>providers/amazon/tests/system/amazon/aws/example_appflow.py:102:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L182'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:182:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_athena.py#L159'>providers/amazon/tests/system/amazon/aws/example_athena.py:159:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L65'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:65:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_batch.py#L258'>providers/amazon/tests/system/amazon/aws/example_batch.py:258:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L108'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:108:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L137'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:137:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L197'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:197:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py#L167'>providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py:167:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L111'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:111:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L112'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:112:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L567'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:567:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_cloudformation.py#L101'>providers/amazon/tests/system/amazon/aws/example_cloudformation.py:101:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L118'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:118:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L74'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:74:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L109'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:109:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L198'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:198:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_datasync.py#L217'>providers/amazon/tests/system/amazon/aws/example_datasync.py:217:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dms.py#L408'>providers/amazon/tests/system/amazon/aws/example_dms.py:408:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dms_serverless.py#L345'>providers/amazon/tests/system/amazon/aws/example_dms_serverless.py:345:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb.py#L109'>providers/amazon/tests/system/amazon/aws/example_dynamodb.py:109:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L154'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:154:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L234'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:234:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_ec2.py#L187'>providers/amazon/tests/system/amazon/aws/example_ec2.py:187:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
... 1911 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 1827 | 1827 | 0 | 0 | 0 |
| AIR301 | 134 | 0 | 134 | 0 | 0 |

</p>
</details>




---

_Renamed from "Add air311 rules" to "[`airflow`] extend `AIR311` rules" by @Lee-W on 2025-04-16 10:08_

---

_Renamed from "[`airflow`] extend `AIR311` rules" to "[ `airflow` ] extend `AIR311` rules" by @Lee-W on 2025-04-16 10:08_

---

_Marked ready for review by @Lee-W on 2025-04-16 10:08_

---

_@ntBre approved on 2025-04-16 16:39_

Thanks, this one looks good to me!

---

_Label `rule` added by @ntBre on 2025-04-16 16:39_

---

_Label `preview` added by @ntBre on 2025-04-16 16:39_

---

_Renamed from "[ `airflow` ] extend `AIR311` rules" to "[`airflow`] extend `AIR311` rules" by @ntBre on 2025-04-16 16:39_

---

_Renamed from "[`airflow`] extend `AIR311` rules" to "[`airflow`] Extend `AIR311` rules" by @ntBre on 2025-04-16 16:39_

---

_Merged by @ntBre on 2025-04-16 16:40_

---

_Closed by @ntBre on 2025-04-16 16:40_

---
