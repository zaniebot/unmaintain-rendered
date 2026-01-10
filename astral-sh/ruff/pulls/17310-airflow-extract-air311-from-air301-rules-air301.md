```yaml
number: 17310
title: "[`airflow`] Extract `AIR311` from `AIR301` rules (`AIR301`, `AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extract-AIR301-to-AIR311
created_at: 2025-04-09T11:07:22Z
updated_at: 2025-04-16T15:06:58Z
url: https://github.com/astral-sh/ruff/pull/17310
synced_at: 2026-01-10T19:33:02Z
```

# [`airflow`] Extract `AIR311` from `AIR301` rules (`AIR301`, `AIR311`)

---

_Pull request opened by @Lee-W on 2025-04-09 11:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

As discussed in https://github.com/astral-sh/ruff/issues/14626#issuecomment-2766146129, we're to separate suggested changes from required changes.

The following symbols have been moved to AIR311 from AIR301. They still work in Airflow 3.0, but they're suggested to be changed as they're expected to be removed in a future version.

* arguments
    * `airflow..DAG | dag`
        * `sla_miss_callback`
    * operators
        * `sla`
* name
    * `airflow.Dataset] | [airflow.datasets.Dataset` → `airflow.sdk.Asset`
    * `airflow.datasets, rest @ ..`
        * `DatasetAlias` → `airflow.sdk.AssetAlias`
        * `DatasetAll` → `airflow.sdk.AssetAll`
        * `DatasetAny` → `airflow.sdk.AssetAny`
        * `expand_alias_to_datasets` → `airflow.sdk.expand_alias_to_assets`
        * `metadata.Metadata` → `airflow.sdk.Metadata`
    <!--airflow.models.baseoperator-->
    * `airflow.models.baseoperator.chain` → `airflow.sdk.chain`
    * `airflow.models.baseoperator.chain_linear` → `airflow.sdk.chain_linear`
    * `airflow.models.baseoperator.cross_downstream` → `airflow.sdk.cross_downstream`
    * `airflow.models.baseoperatorlink.BaseOperatorLink` → `airflow.sdk.definitions.baseoperatorlink.BaseOperatorLink`
    * `airflow.timetables, rest @ ..`
        * `datasets.DatasetOrTimeSchedule` → * `airflow.timetables.assets.AssetOrTimeSchedule`
    * `airflow.utils, rest @ ..`
        <!--airflow.utils.dag_parsing_context-->
        * `dag_parsing_context.get_parsing_context` → `airflow.sdk.get_parsing_context`

## Test Plan

<!-- How was it tested? -->

The test fixture has been updated acccordingly


---

_Renamed from "Extract air301 to air311" to "feat(AIR311): move suggested changes to AIR311" by @Lee-W on 2025-04-09 11:12_

---

_Renamed from "feat(AIR311): move suggested changes to AIR311" to "[`airflow`] Extract `AIR311` from `AIR301` rules (`AIR301`, `AIR311`)" by @Lee-W on 2025-04-10 15:34_

---

_Marked ready for review by @Lee-W on 2025-04-10 15:35_

---

_Comment by @github-actions[bot] on 2025-04-10 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+134 -134 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+134 -134 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_appflow.py#L102'>providers/amazon/tests/system/amazon/aws/example_appflow.py:102:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_appflow.py#L102'>providers/amazon/tests/system/amazon/aws/example_appflow.py:102:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L182'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:182:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L182'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:182:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_athena.py#L159'>providers/amazon/tests/system/amazon/aws/example_athena.py:159:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_athena.py#L159'>providers/amazon/tests/system/amazon/aws/example_athena.py:159:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L65'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:65:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L65'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:65:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_batch.py#L258'>providers/amazon/tests/system/amazon/aws/example_batch.py:258:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_batch.py#L258'>providers/amazon/tests/system/amazon/aws/example_batch.py:258:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L108'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:108:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L108'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:108:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L137'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:137:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L137'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:137:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L197'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:197:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L197'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:197:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py#L167'>providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py:167:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py#L167'>providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py:167:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L111'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:111:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L111'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:111:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L112'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:112:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L112'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:112:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L567'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:567:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L567'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:567:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_cloudformation.py#L101'>providers/amazon/tests/system/amazon/aws/example_cloudformation.py:101:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_cloudformation.py#L101'>providers/amazon/tests/system/amazon/aws/example_cloudformation.py:101:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L118'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:118:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L118'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:118:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L74'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:74:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L74'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:74:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L109'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:109:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L109'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:109:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L198'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:198:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L198'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:198:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_datasync.py#L217'>providers/amazon/tests/system/amazon/aws/example_datasync.py:217:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_datasync.py#L217'>providers/amazon/tests/system/amazon/aws/example_datasync.py:217:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dms.py#L408'>providers/amazon/tests/system/amazon/aws/example_dms.py:408:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dms.py#L408'>providers/amazon/tests/system/amazon/aws/example_dms.py:408:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dms_serverless.py#L345'>providers/amazon/tests/system/amazon/aws/example_dms_serverless.py:345:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dms_serverless.py#L345'>providers/amazon/tests/system/amazon/aws/example_dms_serverless.py:345:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb.py#L109'>providers/amazon/tests/system/amazon/aws/example_dynamodb.py:109:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb.py#L109'>providers/amazon/tests/system/amazon/aws/example_dynamodb.py:109:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L154'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:154:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L154'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:154:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L234'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:234:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L234'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:234:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_ec2.py#L187'>providers/amazon/tests/system/amazon/aws/example_ec2.py:187:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_ec2.py#L187'>providers/amazon/tests/system/amazon/aws/example_ec2.py:187:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_ecs.py#L199'>providers/amazon/tests/system/amazon/aws/example_ecs.py:199:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/86de25fa987b8a48aa922045c23ab82889f55a71/providers/amazon/tests/system/amazon/aws/example_ecs.py#L199'>providers/amazon/tests/system/amazon/aws/example_ecs.py:199:5:</a> AIR311 `airflow.models.baseoperator.chain` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 218 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 134 | 134 | 0 | 0 | 0 |
| AIR301 | 134 | 0 | 134 | 0 | 0 |

</p>
</details>




---

_Comment by @Lee-W on 2025-04-10 15:57_

Hey @ntBre , I might have some refactoring on this one but would appreciate it if we could get an early review to see if I'm doing things right. Thanks 🙂 

---

_Comment by @Lee-W on 2025-04-12 01:52_

@ntBre After a second check, I think this is ready to be reviewed. Thanks!

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:233 on 2025-04-14 16:43_

nit: should this just be `airflow_3_suggested_update_expr` or is there a chance for more of these for minor versions? `airflow_3_1_...` for example

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/helpers.rs`:87 on 2025-04-14 16:51_

It looks like this function is only used once. Are you anticipating using it more in the future? If not, I might lean toward just inlining the additional two arguments to `is_airflow_builtin_or_provider` at the one call site.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:64 on 2025-04-14 16:54_

Should this have a ` \` too? I think this will include a newline. I didn't see any extra newlines in the tests, though, so maybe I'm wrong.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:158 on 2025-04-14 17:09_

nit: we might want to store the output of `segments()` since it's used here and also in the second arm below.

Actually, I guess you can just replace the `_` in the second match arm with a name like `segments` to handle this.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR311_AIR311_args.py.snap`:7 on 2025-04-14 17:14_

Should these diagnostics say `AIR311` instead of `AIR301`?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:89 on 2025-04-14 17:20_

very minor nit: I'd include a newline between the end of the previous function and the next one, here and in a few other places.

---

_@ntBre approved on 2025-04-14 17:20_

Thanks! Just some minor nits, but this looks great overall.

---

_@Lee-W reviewed on 2025-04-15 12:39_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:233 on 2025-04-15 12:39_

No, but these suggested updates might no longer be just suggested updates for versions like 3.1

---

_@Lee-W reviewed on 2025-04-15 13:02_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:87 on 2025-04-15 13:02_

sounds good. updated

---

_@Lee-W reviewed on 2025-04-15 13:08_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:64 on 2025-04-15 13:08_

After a second thought, I think not having breakline's good. so I remove these `\`

---

_@Lee-W reviewed on 2025-04-15 13:25_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:64 on 2025-04-15 13:25_

Tested with a few more formats. I decided to add them back to avoig newlines

---

_@Lee-W reviewed on 2025-04-15 13:37_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:158 on 2025-04-15 13:37_

> nit: we might want to store the output of segments() since it's used here and also in the second arm below.

just extracted it

> Actually, I guess you can just replace the _ in the second match arm with a name like segments to handle this.

I'm not sure I get this part 🤔  could you please elaborate a bit more

---

_@Lee-W reviewed on 2025-04-15 13:43_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR311_AIR311_args.py.snap`:7 on 2025-04-15 13:43_

fixed. Thanks!

---

_@ntBre reviewed on 2025-04-15 13:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:158 on 2025-04-15 13:54_

I should have included a suggestion, but I didn't want to reselect my line range on GitHub :laughing: 

Instead of something like this (which I'm guessing you meant by extracting):

```rust
let segments = qualified_name.segments();
match segments { ... }
```

You can use the `match` pattern to bind `segments`:

```rust
match qualified_name.segments() {
    ["airflow", ..] => ...,
    segments => if is_airflow_operator(segments) {
                checker.report_diagnostics(diagnostic_for_argument(arguments, "sla", None));
            }
}
```

Either way is fine, but that might be slightly neater than extracting the variable like normal.

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:158 on 2025-04-15 14:14_

just fixed it. this looks nice!

---

_@Lee-W reviewed on 2025-04-15 14:14_

---

_@Lee-W reviewed on 2025-04-15 14:16_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:89 on 2025-04-15 14:16_

Updated!

---

_Comment by @Lee-W on 2025-04-15 14:18_

btw do you know when will the next version be released?

---

_Comment by @ntBre on 2025-04-15 14:30_

> btw do you know when will the next version be released?

Should be this Thursday!

---

_Label `rule` added by @dhruvmanila on 2025-04-15 14:32_

---

_Label `preview` added by @dhruvmanila on 2025-04-15 14:32_

---

_Comment by @ntBre on 2025-04-15 19:13_

Is this ready for one more quick review and then merging? I see the comments are resolved, but I don't see any new commits. Maybe GitHub is showing things out of order, though.

---

_Comment by @Lee-W on 2025-04-15 23:24_

@ntBre yep, this is ready. I guess I might have fixed up some commits (but there should still have one or two new commits 🤔

---

_Comment by @Lee-W on 2025-04-15 23:34_

in case of anything missed, i just rebase from the latest main and pushed again

---

_@ntBre reviewed on 2025-04-16 14:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:64 on 2025-04-16 14:05_

That's good. I think some other parts of the code (maybe concise output?) expect these to be single lines.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR311_AIR311_args.py.snap`:4 on 2025-04-16 14:06_

This one still says 301, but the filename says 311. Is that right?

---

_@ntBre approved on 2025-04-16 14:15_

Thanks! I have one more question about a rule code. Is there somewhere that both `Airflow3SuggestedUpdate` and `Airflow3Removal` are being emitted? I'm a bit confused how both rules are active at once since the `test_case` should only be activating `Airflow3SuggestedUpdate`.

---

_@Lee-W reviewed on 2025-04-16 14:27_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR311_AIR311_args.py.snap`:4 on 2025-04-16 14:27_

Oh... this is correct, but probably shouldn't be added here. The test case is supposed to check `sla` instead. let me change it a bit

---

_@Lee-W reviewed on 2025-04-16 14:48_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR311_AIR311_args.py.snap`:4 on 2025-04-16 14:48_

spot one bug and refactor the existing rule with the suggestion in this PR. just updated

---

_@ntBre approved on 2025-04-16 15:06_

---

_Merged by @ntBre on 2025-04-16 15:06_

---

_Closed by @ntBre on 2025-04-16 15:06_

---
