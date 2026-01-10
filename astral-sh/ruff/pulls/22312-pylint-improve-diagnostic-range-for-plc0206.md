```yaml
number: 22312
title: "[`pylint`] Improve diagnostic range for `PLC0206`"
type: pull_request
state: merged
author: ntBre
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: brent/plc0206
created_at: 2025-12-30T22:58:48Z
updated_at: 2025-12-31T18:55:00Z
url: https://github.com/astral-sh/ruff/pull/22312
synced_at: 2026-01-10T16:36:19Z
```

# [`pylint`] Improve diagnostic range for `PLC0206`

---

_Pull request opened by @ntBre on 2025-12-30 22:58_

Summary
--

This PR fixes #14900 by:

- Restricting the diagnostic range from the whole `for` loop to only the `target in iter` part
- Adding secondary annotations to each use of the `dict[key]` accesses
- Adding a `fix_title` suggesting to use `for key in dict.items()`

I thought this approach sounded slightly nicer than the alternative of renaming the rule to focus on each indexing operation mentioned in https://github.com/astral-sh/ruff/issues/14900#issuecomment-2543923625, but I don't feel too strongly. This was easy to implement with our new diagnostic infrastructure too.

This produces an example annotation like this:

```
PLC0206 Extracting value from dictionary without calling `.items()`
  --> dict_index_missing_items.py:59:5
   |
58 | # A case with multiple uses of the value to show off the secondary annotations
59 | for instrument in ORCHESTRA:
   |     ^^^^^^^^^^^^^^^^^^^^^^^
60 |     data = json.dumps(
61 |         {
62 |             "instrument": instrument,
63 |             "section": ORCHESTRA[instrument],
   |                        ---------------------
64 |         }
65 |     )
66 |
67 |     print(f"saving data for {instrument} in {ORCHESTRA[instrument]}")
   |                                              ---------------------
68 |
69 |     with open(f"{instrument}/{ORCHESTRA[instrument]}.txt", "w") as f:
   |                               ---------------------
70 |         f.write(data)
   |
help: Use `for instrument, value in ORCHESTRA.items()` instead
```

which I think is a big improvement over:

```
PLC0206 Extracting value from dictionary without calling `.items()`
  --> dict_index_missing_items.py:59:1
   |
58 |   # A case with multiple uses of the value to show off the secondary annotations
59 | / for instrument in ORCHESTRA:
60 | |     data = json.dumps(
61 | |         {
62 | |             "instrument": instrument,
63 | |             "section": ORCHESTRA[instrument],
64 | |         }
65 | |     )
66 | |
67 | |     print(f"saving data for {instrument} in {ORCHESTRA[instrument]}")
68 | |
69 | |     with open(f"{instrument}/{ORCHESTRA[instrument]}.txt", "w") as f:
70 | |         f.write(data)
   | |_____________________^
   |
```

The secondary annotation feels a bit bare without a message, but I thought it
might be too busy to include one. Something like `value extracted here` or
`indexed here` might work if we do want to include a brief message.

To avoid collecting a `Vec` of annotation ranges, I added a `&Checker` to the
rule's visitor to emit diagnostics as we go instead of at the end.

Test Plan
--

Existing tests, plus a new case showing off multiple secondary annotations


---

_Label `preview` added by @ntBre on 2025-12-30 22:59_

---

_Label `diagnostics` added by @ntBre on 2025-12-30 22:59_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 23:06_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+20 -20 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1474'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1474:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1474'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1474:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:17:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/superset/jinja_context.py#L546'>superset/jinja_context.py:546:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/superset/jinja_context.py#L546'>superset/jinja_context.py:546:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0206 | 40 | 20 | 20 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+20 -20 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1474'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1474:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L1474'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:1474:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/airflow-core/tests/unit/serialization/test_dag_serialization.py#L682'>airflow-core/tests/unit/serialization/test_dag_serialization.py:682:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/devel-common/src/docs/utils/conf_constants.py#L208'>devel-common/src/docs/utils/conf_constants.py:208:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L136'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:136:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py#L740'>providers/amazon/tests/unit/amazon/aws/executors/batch/test_batch_executor.py:740:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L1329'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:1329:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/providers/standard/tests/unit/standard/operators/test_hitl.py#L332'>providers/standard/tests/unit/standard/operators/test_hitl.py:332:17:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/77b200153f71b12b1d6d79a3c0b7b32a150acdea/scripts/ci/prek/check_shared_distributions_usage.py#L409'>scripts/ci/prek/check_shared_distributions_usage.py:409:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/superset/jinja_context.py#L546'>superset/jinja_context.py:546:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/superset/jinja_context.py#L546'>superset/jinja_context.py:546:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/apache/superset/blob/85e830de46eea3dcd4959908782f8db03c5dfd89/tests/integration_tests/reports/api_tests.py#L301'>tests/integration_tests/reports/api_tests.py:301:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/aws/aws-sam-cli/blob/eb1da4888be5b628916d6a02f55ae3babb4dfd4c/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0206 | 40 | 20 | 20 | 0 | 0 |

</p>
</details>





---

_Marked ready for review by @ntBre on 2025-12-30 23:13_

---

_Review requested from @MichaReiser by @ntBre on 2025-12-30 23:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLC0206_dict_index_missing_items.py.snap`:122 on 2025-12-31 08:02_

Part of the confusion in the original issue was that it wasn't clear how to fix the violation. Could we add an info message sub-diagnostic suggesting the use of `Use for instrument, value in ORCHESTRA.items()` instead?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLC0206_dict_index_missing_items.py.snap`:17 on 2025-12-31 08:06_

Is it necessary to make this a preview-only change? To suppress PLC0206 today, you have to put the noqa on the line where the `for` statement starts. Will this change with this PR?

The only example that I can think of is:

```py
ORCHESTRA = dict()

for (  # noqa PLC0206
    instrument
) in ORCHESTRA:
    print(f"{instrument}: {ORCHESTRA[instrument]}")


```

But we could fix this by using `parenthesized_range` for the for-target. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/dict_index_missing_items.rs`:91 on 2025-12-31 08:07_

Do we need the two lifetimes or would using `'a` everywhere be sufficient?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/dict_index_missing_items.rs`:84 on 2025-12-31 08:09_

I suggest moving this into `SubscriptVisitor::new`, as it seems the better place once we remove the preview gating.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/dict_index_missing_items.rs`:158 on 2025-12-31 08:10_

```suggestion
						let guard = self.guard.get_or_init(|| self.checker
                        .report_diagnostic(DictIndexMissingItems, self.range)
          	};

            if is_plc0206_narrower_range_enabled(self.checker.settings()) {
          	    guard.secondary_annotation("", expr);
            }
```

---

_@MichaReiser approved on 2025-12-31 08:10_

Nice

---

_@ntBre reviewed on 2025-12-31 13:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLC0206_dict_index_missing_items.py.snap`:17 on 2025-12-31 13:54_

Oh, I guess you're right! I saw the discussion about the breaking change in the issue, but that must have been in case we moved the diagnostic range to the indexing lines. I think that means I can drop the preview checks entirely?

---

_@MichaReiser reviewed on 2025-12-31 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLC0206_dict_index_missing_items.py.snap`:17 on 2025-12-31 14:01_

I think so, at least if the suppression ranges indeed remain unchanged (see my example above)

---

_@ntBre reviewed on 2025-12-31 14:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLC0206_dict_index_missing_items.py.snap`:17 on 2025-12-31 14:03_

Yep, I tested out your example and added it as a test. I'll try to come up with any other tricky cases, but that seems to cover what I've tried so far.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_index_missing_items.rs`:91 on 2025-12-31 14:13_

I tried that first and just checked again, and I believe we do need both here. The compiler wants me to start annotating the parent functions if I use just `'a`.

<details><summary>Details</summary>
<p>

```
error: lifetime may not live long enough
  --> crates/ruff_linter/src/rules/pylint/rules/dict_index_missing_items.rs:80:5
   |
62 | pub(crate) fn dict_index_missing_items(checker: &Checker, stmt_for: &ast::StmtFor) {
   |                                        -------  - let's call the lifetime of this reference `'1`
   |                                        |
   |                                        has type `&Checker<'2>`
...
80 |     SubscriptVisitor::new(stmt_for, dict_name, checker).visit_body(body);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ argument requires that `'1` must outlive `'2`
   |
   = note: requirement occurs because of the type `Checker<'_>`, which makes the generic argument `'_` invariant
   = note: the struct `Checker<'a>` is invariant over the parameter `'a`
   = help: see <https://doc.rust-lang.org/nomicon/subtyping.html> for more information about variance
help: consider introducing a named lifetime parameter
   |
62 | pub(crate) fn dict_index_missing_items<'a>(checker: &'a Checker<'a>, stmt_for: &ast::StmtFor) {
   |                                       ++++           ++        ++++

error: lifetime may not live long enough
  --> crates/ruff_linter/src/rules/pylint/rules/dict_index_missing_items.rs:80:5
   |
62 | pub(crate) fn dict_index_missing_items(checker: &Checker, stmt_for: &ast::StmtFor) {
   |                                        -------                      - let's call the lifetime of this reference `'3`
   |                                        |
   |                                        has type `&Checker<'2>`
...
80 |     SubscriptVisitor::new(stmt_for, dict_name, checker).visit_body(body);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ argument requires that `'3` must outlive `'2`
   |
   = note: requirement occurs because of the type `Checker<'_>`, which makes the generic argument `'_` invariant
   = note: the struct `Checker<'a>` is invariant over the parameter `'a`
   = help: see <https://doc.rust-lang.org/nomicon/subtyping.html> for more information about variance
help: consider introducing a named lifetime parameter
   |
62 | pub(crate) fn dict_index_missing_items<'a>(checker: &Checker<'a>, stmt_for: &'a ast::StmtFor) {
   |                                       ++++                  ++++             ++
```

</p>
</details> 

---

_@ntBre reviewed on 2025-12-31 14:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLC0206_dict_index_missing_items.py.snap`:122 on 2025-12-31 14:14_

Oh yeah, that seems like a nice `fix_title` even without a fix.

---

_@ntBre reviewed on 2025-12-31 14:14_

---

_Label `preview` removed by @ntBre on 2025-12-31 14:36_

---

_Merged by @ntBre on 2025-12-31 18:54_

---

_Closed by @ntBre on 2025-12-31 18:54_

---

_Branch deleted on 2025-12-31 18:55_

---
