```yaml
number: 17369
title: Adds profile guided optimization script.
type: pull_request
state: open
author: FilipAndersson245
labels:
  - performance
assignees: []
base: main
head: pgo-script
created_at: 2025-04-12T20:11:54Z
updated_at: 2025-09-15T09:08:23Z
url: https://github.com/astral-sh/ruff/pull/17369
synced_at: 2026-01-12T15:56:01Z
```

# Adds profile guided optimization script.

---

_@FilipAndersson245_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds a script to generate a pgo optimized binary of ruff, see #7055 for more detail.
I initially only implemented the script on `x86_64-unknown-linux-gnu` so people can try it out, if we think this is more interesting it should probably be added to the release pipeline. from what I can see you get a general 3-10% improvement of runtime speed then running the pgo-binaries, as well as a somewhat smaller binary size. 

The script fetches a large popular python codebases and uses that as training data (see `pgo_profile.json`),

this is an example of the performance for a cold run on pytorch's source, where the pgo is 12% faster.
```
✦ ❯ hyperfine "./target/x86_64-unknown-linux-gnu/release/ruff check -e -n  clones/pytorch/" "./target/release/ruff check -e -n  clones/pytorch/" --warmup 100 -r 1000
Benchmark 1: ./target/x86_64-unknown-linux-gnu/release/ruff check -e -n  clones/pytorch/
  Time (mean ± σ):     169.4 ms ±   6.4 ms    [User: 2942.4 ms, System: 244.4 ms]
  Range (min … max):   151.0 ms … 212.7 ms    1000 runs

Benchmark 2: ./target/release/ruff check -e -n  clones/pytorch/
  Time (mean ± σ):     190.5 ms ±   7.5 ms    [User: 3641.0 ms, System: 239.7 ms]
  Range (min … max):   168.1 ms … 235.4 ms    1000 runs

Summary
  ./target/x86_64-unknown-linux-gnu/release/ruff check -e -n  clones/pytorch/ ran
    1.12 ± 0.06 times faster than ./target/release/ruff check -e -n  clones/pytorch/
```

## Test Plan

I tested this by running
ensure you have [cargo-pgo](https://github.com/Kobzol/cargo-pgo) installed and configured.
run `python ./scripts/pgo.py`


---

_Comment by @FilipAndersson245 on 2025-04-21 11:03_

@MichaReiser could you checkout if this is a valid start for pgo? 

---

_Label `performance` added by @dhruvmanila on 2025-04-21 16:03_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-04-21 16:03_

---

_Comment by @github-actions[bot] on 2025-04-21 16:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+142 -1834 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+138 -1832 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L106'>airflow-core/src/airflow/api/common/mark_tasks.py:106:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L142'>airflow-core/src/airflow/api/common/mark_tasks.py:142:22:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L195'>airflow-core/src/airflow/api/common/mark_tasks.py:195:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L239'>airflow-core/src/airflow/api/common/mark_tasks.py:239:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L325'>airflow-core/src/airflow/api/common/mark_tasks.py:325:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L356'>airflow-core/src/airflow/api/common/mark_tasks.py:356:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api/common/mark_tasks.py#L87'>airflow-core/src/airflow/api/common/mark_tasks.py:87:32:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/datamodels/dag_run.py#L108'>airflow-core/src/airflow/api_fastapi/core_api/datamodels/dag_run.py:108:37:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/assets.py#L320'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/assets.py:320:10:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L159'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:159:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L263'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:263:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L337'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:337:14:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py#L401'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_run.py:401:14:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_versions.py#L109'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dag_versions.py:109:14:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py#L170'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py:170:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py#L197'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/dags.py:197:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/airflow-core/src/airflow/api_fastapi/core_api/routes/public/extra_links.py#L58'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/extra_links.py:58:10:</a> AIR311 `airflow.models.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 1812 additional changes omitted for rule AIR311
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_appflow.py#L102'>providers/amazon/tests/system/amazon/aws/example_appflow.py:102:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_appflow_run.py#L182'>providers/amazon/tests/system/amazon/aws/example_appflow_run.py:182:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_athena.py#L159'>providers/amazon/tests/system/amazon/aws/example_athena.py:159:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py#L65'>providers/amazon/tests/system/amazon/aws/example_azure_blob_to_s3.py:65:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_batch.py#L258'>providers/amazon/tests/system/amazon/aws/example_batch.py:258:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L108'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:108:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L137'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:137:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L197'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:197:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py#L167'>providers/amazon/tests/system/amazon/aws/example_bedrock_batch_inference.py:167:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L111'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:111:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L112'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:112:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py#L567'>providers/amazon/tests/system/amazon/aws/example_bedrock_retrieve_and_generate.py:567:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_cloudformation.py#L101'>providers/amazon/tests/system/amazon/aws/example_cloudformation.py:101:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L118'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:118:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_comprehend.py#L74'>providers/amazon/tests/system/amazon/aws/example_comprehend.py:74:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L109'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:109:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py#L198'>providers/amazon/tests/system/amazon/aws/example_comprehend_document_classifier.py:198:5:</a> AIR301 `airflow.models.baseoperator.chain` is removed in Airflow 3.0
... 118 additional changes omitted for rule AIR301
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L141'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:141:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L141'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:141:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/exasol/src/airflow/providers/exasol/hooks/exasol.py#L70'>providers/exasol/src/airflow/providers/exasol/hooks/exasol.py:70:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/exasol/src/airflow/providers/exasol/hooks/exasol.py#L70'>providers/exasol/src/airflow/providers/exasol/hooks/exasol.py:70:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/mysql/src/airflow/providers/mysql/hooks/mysql.py#L191'>providers/mysql/src/airflow/providers/mysql/hooks/mysql.py:191:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/mysql/src/airflow/providers/mysql/hooks/mysql.py#L191'>providers/mysql/src/airflow/providers/mysql/hooks/mysql.py:191:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/postgres/src/airflow/providers/postgres/hooks/postgres.py#L171'>providers/postgres/src/airflow/providers/postgres/hooks/postgres.py:171:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/e4957ff3827e0aea0465026023dd58288c5b1299/providers/postgres/src/airflow/providers/postgres/hooks/postgres.py#L171'>providers/postgres/src/airflow/providers/postgres/hooks/postgres.py:171:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
... 1928 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py#L122'>superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py:122:21:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py#L122'>superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py:122:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L795'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:795:21:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L795'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:795:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/31b073c794507204c8aa43e0770bb816d43c4f81/pandas/tests/arrays/sparse/test_accessor.py#L247'>pandas/tests/arrays/sparse/test_accessor.py:247:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/31b073c794507204c8aa43e0770bb816d43c4f81/pandas/tests/arrays/sparse/test_accessor.py#L96'>pandas/tests/arrays/sparse/test_accessor.py:96:50:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 1828 | 0 | 1828 | 0 | 0 |
| AIR301 | 134 | 134 | 0 | 0 | 0 |
| PERF403 | 12 | 6 | 6 | 0 | 0 |
| RUF043 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @FilipAndersson245 on 2025-04-25 07:13_

Anything more that should be fixed? 

Was thinking if this works out good we could test adding it to the release pipeline. 

---

_Comment by @MichaReiser on 2025-04-25 07:19_

Sorry, I haven't had time to look at this yet (I was out all last week and we're getting close to an important release. That's why my time is a bit limited at the moment). But this PR is still on my todo list

---

_Comment by @FilipAndersson245 on 2025-04-25 07:23_

No problem, just didn't want it to be forgotten :) 

---

_Comment by @FilipAndersson245 on 2025-05-20 19:28_

Any update on when this may be reviewed?

---

_Comment by @MichaReiser on 2025-05-20 20:30_

Sorry, I'm very busy at the moment. It's on my list but I don't know when I'll get to it

---

_Comment by @MichaReiser on 2025-05-20 20:30_

Sorry, I'm very busy at the moment. It's on my list but I don't know when I'll get to it

---

_Comment by @FilipAndersson245 on 2025-07-03 05:31_

Are you still interested or should we close this? 

---

_Comment by @MeGaGiGaGon on 2025-07-03 05:56_

I saw ntBre say in another issue he’s off on PTO for a week, so you might not get a response till then

---

_Comment by @ntBre on 2025-07-03 14:39_

I think we're definitely still interested, just need to find enough time to review it and think about how we'd want to incorporate it into our release process, as Micha mentioned on the [issue](https://github.com/astral-sh/ruff/issues/7055#issuecomment-2797180194).

Thank you for your work on this!

---

_Comment by @FilipAndersson245 on 2025-09-07 10:24_

This PR is now more then 5 month old so I guess it only adds noise at this point. 

---

_Comment by @MichaReiser on 2025-09-15 09:08_

I still find it a valuable source to understand how to add a PGO script. 

---
