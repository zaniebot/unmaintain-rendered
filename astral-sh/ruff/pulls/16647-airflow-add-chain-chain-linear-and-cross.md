```yaml
number: 16647
title: "[`airflow`] Add `chain`, `chain_linear` and `cross_downstream` for `AIR302`"
type: pull_request
state: merged
author: kaxil
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-more-airflow-imports
created_at: 2025-03-11T19:55:23Z
updated_at: 2025-03-18T05:38:46Z
url: https://github.com/astral-sh/ruff/pull/16647
synced_at: 2026-01-12T15:55:55Z
```

# [`airflow`] Add `chain`, `chain_linear` and `cross_downstream` for `AIR302`

---

_@kaxil_

Similar to https://github.com/astral-sh/ruff/pull/16014. PR on Airflow side: https://github.com/apache/airflow/pull/47639

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Similar to https://github.com/astral-sh/ruff/pull/16014. PR on Airflow side: https://github.com/apache/airflow/pull/47639


## Test Plan

<!-- How was it tested? -->
a test fixture has been updated

cc @Lee-W @dhruvmanila

---

_Comment by @github-actions[bot] on 2025-03-11 20:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+153 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+153 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_asset_with_watchers.py#L45'>airflow/example_dags/example_asset_with_watchers.py:45:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_bash_decorator.py#L112'>airflow/example_dags/example_bash_decorator.py:112:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_bash_decorator.py#L113'>airflow/example_dags/example_bash_decorator.py:113:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_complex.py#L160'>airflow/example_dags/example_complex.py:160:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_complex.py#L190'>airflow/example_dags/example_complex.py:190:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_complex.py#L214'>airflow/example_dags/example_complex.py:214:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_short_circuit_decorator.py#L42'>airflow/example_dags/example_short_circuit_decorator.py:42:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_short_circuit_decorator.py#L43'>airflow/example_dags/example_short_circuit_decorator.py:43:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_short_circuit_decorator.py#L57'>airflow/example_dags/example_short_circuit_decorator.py:57:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_short_circuit_operator.py#L51'>airflow/example_dags/example_short_circuit_operator.py:51:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_short_circuit_operator.py#L52'>airflow/example_dags/example_short_circuit_operator.py:52:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/airflow/example_dags/example_short_circuit_operator.py#L66'>airflow/example_dags/example_short_circuit_operator.py:66:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/dev/perf/dags/elastic_dag.py#L194'>dev/perf/dags/elastic_dag.py:194:26:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/performance/src/performance_dags/performance_dag/performance_dag.py#L259'>performance/src/performance_dags/performance_dag/performance_dag.py:259:26:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_appflow.py#L101'>providers/amazon/tests/system/amazon/aws/example_appflow.py:101:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L181'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:181:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_athena.py#L158'>providers/amazon/tests/system/amazon/aws/example_athena.py:158:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L64'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:64:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_batch.py#L257'>providers/amazon/tests/system/amazon/aws/example_batch.py:257:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L112'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:112:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L113'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:113:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L145'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:145:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L146'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:146:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L207'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:207:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L110'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:110:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L111'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:111:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L566'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:566:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_cloudformation.py#L100'>providers/amazon/tests/system/amazon/aws/example_cloudformation.py:100:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L117'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:117:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L73'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:73:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L108'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:108:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L197'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:197:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_datasync.py#L216'>providers/amazon/tests/system/amazon/aws/example_datasync.py:216:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_dms.py#L407'>providers/amazon/tests/system/amazon/aws/example_dms.py:407:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_dms_serverless.py#L344'>providers/amazon/tests/system/amazon/aws/example_dms_serverless.py:344:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_dynamodb.py#L108'>providers/amazon/tests/system/amazon/aws/example_dynamodb.py:108:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L160'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:160:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L161'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:161:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py#L241'>providers/amazon/tests/system/amazon/aws/example_dynamodb_to_s3.py:241:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_ec2.py#L186'>providers/amazon/tests/system/amazon/aws/example_ec2.py:186:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_ecs.py#L198'>providers/amazon/tests/system/amazon/aws/example_ecs.py:198:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_ecs_fargate.py#L145'>providers/amazon/tests/system/amazon/aws/example_ecs_fargate.py:145:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_eks_templated.py#L134'>providers/amazon/tests/system/amazon/aws/example_eks_templated.py:134:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_eks_with_fargate_in_one_step.py#L130'>providers/amazon/tests/system/amazon/aws/example_eks_with_fargate_in_one_step.py:130:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_eks_with_fargate_profile.py#L160'>providers/amazon/tests/system/amazon/aws/example_eks_with_fargate_profile.py:160:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_eks_with_nodegroup_in_one_step.py#L142'>providers/amazon/tests/system/amazon/aws/example_eks_with_nodegroup_in_one_step.py:142:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_eks_with_nodegroups.py#L181'>providers/amazon/tests/system/amazon/aws/example_eks_with_nodegroups.py:181:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_emr.py#L206'>providers/amazon/tests/system/amazon/aws/example_emr.py:206:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_emr_eks.py#L299'>providers/amazon/tests/system/amazon/aws/example_emr_eks.py:299:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/85487a8002872baf78ee1cc28eff6443597105bc/providers/amazon/tests/system/amazon/aws/example_emr_notebook_execution.py#L101'>providers/amazon/tests/system/amazon/aws/example_emr_notebook_execution.py:101:5:</a> AIR302 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
... 103 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 153 | 153 | 0 | 0 | 0 |

</p>
</details>




---

_@Lee-W reviewed on 2025-03-12 00:03_

LGTM 👍

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-03-13 11:49_

---

_Label `rule` added by @dhruvmanila on 2025-03-18 05:37_

---

_Label `preview` added by @dhruvmanila on 2025-03-18 05:37_

---

_Renamed from "[airflow] Add `chain`, `chain_linear` and `cross_downstream` for AIR302" to "[`airflow`] Add `chain`, `chain_linear` and `cross_downstream` for `AIR302`" by @dhruvmanila on 2025-03-18 05:38_

---

_@dhruvmanila approved on 2025-03-18 05:38_

Looks good, thanks

---

_Merged by @dhruvmanila on 2025-03-18 05:38_

---

_Closed by @dhruvmanila on 2025-03-18 05:38_

---
