```yaml
number: 12944
title: "[flake8_bugbear] message based on expression location [B015]"
type: pull_request
state: merged
author: dsal3389
labels:
  - rule
assignees: []
merged: true
base: main
head: ruff-b015
created_at: 2024-08-16T22:21:22Z
updated_at: 2024-08-17T11:54:19Z
url: https://github.com/astral-sh/ruff/pull/12944
synced_at: 2026-01-12T15:55:42Z
```

# [flake8_bugbear] message based on expression location [B015]

---

_@dsal3389_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
for issue https://github.com/astral-sh/ruff/issues/12617, display different
message if useless comparison is at the end of function scope.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
```console
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B015.py --preview --no-cache --select B015
```

I also added another function to the test file, to see the message
if the comparison is not at the end of the scope 


---

_Comment by @github-actions[bot] on 2024-08-16 22:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+15 -15 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+15 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/api_connexion/endpoints/test_dag_stats_endpoint.py#L172'>tests/api_connexion/endpoints/test_dag_stats_endpoint.py:172:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/api_connexion/endpoints/test_dag_stats_endpoint.py#L172'>tests/api_connexion/endpoints/test_dag_stats_endpoint.py:172:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L179'>tests/core/test_otel_logger.py:179:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L179'>tests/core/test_otel_logger.py:179:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L198'>tests/core/test_otel_logger.py:198:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L198'>tests/core/test_otel_logger.py:198:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L253'>tests/core/test_otel_logger.py:253:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L253'>tests/core/test_otel_logger.py:253:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/decorators/test_mapped.py#L38'>tests/decorators/test_mapped.py:38:5:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/decorators/test_mapped.py#L38'>tests/decorators/test_mapped.py:38:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/jobs/test_backfill_job.py#L1999'>tests/jobs/test_backfill_job.py:1999:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/jobs/test_backfill_job.py#L1999'>tests/jobs/test_backfill_job.py:1999:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/models/test_taskinstance.py#L572'>tests/models/test_taskinstance.py:572:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/models/test_taskinstance.py#L572'>tests/models/test_taskinstance.py:572:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L112'>tests/providers/amazon/aws/operators/test_emr_serverless.py:112:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L112'>tests/providers/amazon/aws/operators/test_emr_serverless.py:112:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L235'>tests/providers/amazon/aws/operators/test_emr_serverless.py:235:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L235'>tests/providers/amazon/aws/operators/test_emr_serverless.py:235:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L415'>tests/providers/google/cloud/triggers/test_vertex_ai.py:415:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L415'>tests/providers/google/cloud/triggers/test_vertex_ai.py:415:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L555'>tests/providers/google/cloud/triggers/test_vertex_ai.py:555:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L555'>tests/providers/google/cloud/triggers/test_vertex_ai.py:555:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L705'>tests/providers/google/cloud/triggers/test_vertex_ai.py:705:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L705'>tests/providers/google/cloud/triggers/test_vertex_ai.py:705:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L860'>tests/providers/google/cloud/triggers/test_vertex_ai.py:860:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L860'>tests/providers/google/cloud/triggers/test_vertex_ai.py:860:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/serialization/test_dag_serialization.py#L2563'>tests/serialization/test_dag_serialization.py:2563:5:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/serialization/test_dag_serialization.py#L2563'>tests/serialization/test_dag_serialization.py:2563:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/utils/test_task_group.py#L1428'>tests/utils/test_task_group.py:1428:5:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/utils/test_task_group.py#L1428'>tests/utils/test_task_group.py:1428:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B015 | 30 | 15 | 15 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -15 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+15 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/api_connexion/endpoints/test_dag_stats_endpoint.py#L172'>tests/api_connexion/endpoints/test_dag_stats_endpoint.py:172:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/api_connexion/endpoints/test_dag_stats_endpoint.py#L172'>tests/api_connexion/endpoints/test_dag_stats_endpoint.py:172:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L179'>tests/core/test_otel_logger.py:179:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L179'>tests/core/test_otel_logger.py:179:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L198'>tests/core/test_otel_logger.py:198:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L198'>tests/core/test_otel_logger.py:198:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L253'>tests/core/test_otel_logger.py:253:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/core/test_otel_logger.py#L253'>tests/core/test_otel_logger.py:253:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/decorators/test_mapped.py#L38'>tests/decorators/test_mapped.py:38:5:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/decorators/test_mapped.py#L38'>tests/decorators/test_mapped.py:38:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/jobs/test_backfill_job.py#L1999'>tests/jobs/test_backfill_job.py:1999:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/jobs/test_backfill_job.py#L1999'>tests/jobs/test_backfill_job.py:1999:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/models/test_taskinstance.py#L572'>tests/models/test_taskinstance.py:572:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/models/test_taskinstance.py#L572'>tests/models/test_taskinstance.py:572:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L112'>tests/providers/amazon/aws/operators/test_emr_serverless.py:112:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L112'>tests/providers/amazon/aws/operators/test_emr_serverless.py:112:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L235'>tests/providers/amazon/aws/operators/test_emr_serverless.py:235:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/amazon/aws/operators/test_emr_serverless.py#L235'>tests/providers/amazon/aws/operators/test_emr_serverless.py:235:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L415'>tests/providers/google/cloud/triggers/test_vertex_ai.py:415:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L415'>tests/providers/google/cloud/triggers/test_vertex_ai.py:415:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L555'>tests/providers/google/cloud/triggers/test_vertex_ai.py:555:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L555'>tests/providers/google/cloud/triggers/test_vertex_ai.py:555:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L705'>tests/providers/google/cloud/triggers/test_vertex_ai.py:705:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L705'>tests/providers/google/cloud/triggers/test_vertex_ai.py:705:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L860'>tests/providers/google/cloud/triggers/test_vertex_ai.py:860:9:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/providers/google/cloud/triggers/test_vertex_ai.py#L860'>tests/providers/google/cloud/triggers/test_vertex_ai.py:860:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/serialization/test_dag_serialization.py#L2563'>tests/serialization/test_dag_serialization.py:2563:5:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/serialization/test_dag_serialization.py#L2563'>tests/serialization/test_dag_serialization.py:2563:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/utils/test_task_group.py#L1428'>tests/utils/test_task_group.py:1428:5:</a> B015 Pointless comparison at end of function scope. Did you mean to return the expression result?
- <a href='https://github.com/apache/airflow/blob/60fe8c932926a2fd778a82ec1a2ee216f05c6c5f/tests/utils/test_task_group.py#L1428'>tests/utils/test_task_group.py:1428:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B015 | 30 | 15 | 15 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_comparison.rs`:73 on 2024-08-17 11:09_

I think this works but it's a fairly subtle assumption that this will always hold true. We actually considered changing the parser to include the range of the `decent` in the range of the function (which would simplify the formatter by a lot). 

That's why I would prefer a check like this
```suggestion
						let is_last = function
                .body
                .last()
                .and_then(Stmt::as_expr_stmt)
                .is_some_and(|last_stmt| &*last_stmt.value == expr);
            if is_last {
```

---

_@MichaReiser approved on 2024-08-17 11:10_

Thanks. I've a small suggestion to make the code a bit more resilient to AST/parser changes. But this otherwise looks good. Thank you

---

_Label `rule` added by @MichaReiser on 2024-08-17 11:10_

---

_Review comment by @dsal3389 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_comparison.rs`:73 on 2024-08-17 11:43_

Thank you for the suggestion!

---

_@dsal3389 reviewed on 2024-08-17 11:43_

---

_Merged by @MichaReiser on 2024-08-17 11:54_

---

_Closed by @MichaReiser on 2024-08-17 11:54_

---
