```yaml
number: 12919
title: Pull all types in corpus tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: pull-all-types-in-corpus-test
created_at: 2024-08-16T08:14:55Z
updated_at: 2024-08-17T12:09:11Z
url: https://github.com/astral-sh/ruff/pull/12919
synced_at: 2026-01-12T15:55:42Z
```

# Pull all types in corpus tests

---

_@MichaReiser_

## Summary

This PR changes our corpus test to pull all expression and definition types by calling `HasTy::ty`. 

This uncovered a few bugs. I addressed one as part of this PR, but a few are more involved. I added fixme's in the relevant places.

## Test Plan

<!-- How was it tested? -->


---

_@MichaReiser reviewed on 2024-08-16 09:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:725 on 2024-08-16 09:11_

This fixes a bug where the type for the assignment's target expression was missing. 

---

_Label `red-knot` added by @MichaReiser on 2024-08-16 09:16_

---

_Marked ready for review by @MichaReiser on 2024-08-16 09:16_

---

_Review requested from @carljm by @MichaReiser on 2024-08-16 09:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-16 09:16_

---

_Comment by @github-actions[bot] on 2024-08-16 09:28_

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
ℹ️ ecosystem check **detected linter changes**. (+15 -18 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

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
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/ff28a3e8a86fd843d69a7ac99948f9261d076f60/pandas/core/indexes/range.py#L1390'>pandas/core/indexes/range.py:1390:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/ff28a3e8a86fd843d69a7ac99948f9261d076f60/pandas/core/indexes/range.py#L455'>pandas/core/indexes/range.py:455:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/ff28a3e8a86fd843d69a7ac99948f9261d076f60/pandas/core/indexes/range.py#L834'>pandas/core/indexes/range.py:834:16:</a> F841 Local variable `result_name` is assigned to but never used
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B015 | 30 | 15 | 15 | 0 | 0 |
| PLR6104 | 2 | 0 | 2 | 0 | 0 |
| F841 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-08-16 18:18_

It would be great if we could merge this soon. It gives nice test coverage and I worry that new PRs introduce new issues 😅

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:759 on 2024-08-16 18:18_

Yes, this is wrong (there's a lot of things that aren't right yet in type inference; I try to have TODOs in those places but not all of them have it.)

The right approach here is a bit more complex than what you outline, and we'll need to deal with this in the "annotated types" task that's on the roadmap pretty soon.

Currently all our types are "inferred types" (that is, the most precise type we can establish for what the actual value will be at runtime) and we don't pay any attention to annotations.

We need to add the concept of "declared types", which are slightly different. An annotated assignment without RHS (e.g. `x: int`) sets the declared type for `x` to `int` while the inferred type would still be `Unbound`. They can also differ even if there is an RHS, e.g. in `x: int = 1` the declared type is `int` but we would want the inferred type to be `Literal[1]`.

The declared type is mostly relevant for deciding whether later assignments should be valid, or should raise a diagnostic (if you assign a type that is not consistent with the declared type.)

But all of that is for later; for this PR I would add a TODO comment here but not try to describe the entire correct behavior, e.g. just this:
```suggestion
        // TODO correct type inference for annotated assignment
```

Alternatively, it's a bit of scope creep but an easy partial fix here would be to take the type of the optional `value` (if any) and assign that as the type of the target. If you do that, we would still need a TODO for correct handling of the annotation type as declared type, and for the case with no RHS. (Though if you do this partial fix here, I'd also want a test for that type inference.)

(To be honest, until I dig deeper into design for handling declared types, I'm not even sure if annotated assignment with no RHS should be a `Definition` at all.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_model.rs`:174 on 2024-08-16 18:24_

This is true, but I'm not sure its a FIXME. I think it is probably correct for assignments to non-Names to not be Definitions, so I suspect that won't change (though I haven't gone deep into designing this yet.)

If you have e.g. `foo.x = "bar"`, that doesn't define any symbol in the current scope. It assigns a new value to an attribute of a local symbol; the type of that attribute comes from a symbol in a different scope. The simplest and safest approach here is to simply check that the assignment is type-correct, but otherwise don't change anything in the current scope. But we may want to do some type narrowing on attributes based on assignments (e.g. after `foo.x = 1` we can "know" that `foo.x` is `Literal[1]` now, even though that `x` attribute might have a broader type like `int | None`). This attribute narrowing is often unsound (because technically some other code you call might reassign the attribute between when you assigned it and when you later use it), but it may give practically better results on real code to accept that risk and do the narrowing anyway. This narrowing-of-attributes feature will require some design work, but I suspect that it won't make sense to re-use `Definition` for it, because `Definition` is tied to a symbol in the local scope.

So I would adjust this comment:
```suggestion
        // We don't consider assignments with non-name targets as definitions.
```
(I also don't feel like we need the link to the other comment here, the comment it is linking to doesn't give any more information than this comment does.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_model.rs`:170 on 2024-08-16 18:35_

I would prefer not to implement `HasTy` for `StmtAnnAssign` at all, just like we don't implement it for `StmtAssign`.

Implementing a method `.ty(...)` for non-expressions is kind of weird to begin with. For expressions the meaning is always crystal clear: what type describes the values this expression can have at runtime? For non-expressions it's often less obvious. For some nodes (function def, class def) there is a clear and obvious meaning, because the statement always creates a single type. But it's not so clear for e.g. `StmtAnnAssign`. You could consider it to have the type of the RHS expression, or the type of the annotation, but if there are both? Not clear at all.

There's no requirement that we implement `HasTy` for all AST nodes that can form part of a definition; it should only be implemented for ones where it is clear what the meaning should be. For a case like `StmtAnnAssign` I would rather that we just don't implement it, and you have to explicitly ask for the type of the RHS expression or the type of the annotation.

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:43 on 2024-08-16 18:38_

To me the important thing is that every expression has a `.ty` -- non-expression nodes are much more only-if-it-makes-sense, as I discussed in a comment above.

And what we are pulling here is a type for an AST node, so the relevant issue isn't even "do all definitions have a type" it's "which kinds of AST nodes do we want to ensure there is always a type associated with." 

So I'd rather this comment be clearer about that, e.g. something like
```suggestion
        // this test is only asserting that we can pull every expression type without a panic (and some non-expressions that clearly define a single type)
```

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:61 on 2024-08-16 18:40_

I am curious about the future of `SemanticModel` as we switch to a type-checker model. We don't have access to `SemanticModel` in type inference, which is where I think most (if not all) type diagnostics will be raised. So we need to figure out where `SemanticModel` and `HasTy` would even be used.

I think this test is useful because it verifies that type inference isn't missing anything, but other than this test it's not clear to me where we'll end up using the `HasTy` trait (other than in the future when we get to doing a type-aware linter.)

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:98 on 2024-08-16 18:42_

It's not clear whether the "type of an AnnAssign node" should be the type of the RHS (if any) or the type of the annotation, which is why I would prefer to not implement `HasTy` for it (or check it here) at all.

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:145 on 2024-08-16 18:49_

I think the status quo here is not too bad -- we already don't have a contract that all non-expression nodes have a `.ty`. The only weird thing is that `Parameter` does have a `.ty` but sometimes it will panic. That's awkward, but we can manage it if when visiting a parameter in type inference we always pass along the context of whether it's inside a parameter-with-default or not. The cleaner solution would be to have an explicit marker on the Parameter AST node (or a different node type), but that's a more invasive change to the AST.

---

_@carljm approved on 2024-08-16 19:03_

Thanks for updating this test! I think it's really valuable to validate our expression coverage in type inference.

Sorry for the wall-of-text in some of the comments; this PR raises a lot of subtle questions!

---

_Comment by @carljm on 2024-08-16 19:24_

> It would be great if we could merge this soon. It gives nice test coverage and I worry that new PRs introduce new issues 😅

Sorry! Too many PRs to review this morning.

---

_@MichaReiser reviewed on 2024-08-16 20:15_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:61 on 2024-08-16 20:15_

It's still my goal that the lsp doesn't use any salsa queries directly, the same as rust analyzer. The abstraction also make sense to ease our transition when migrating ruff. That's why do think they're still valuable. Also, we prioritize building a type checker first. We haven't transitioned to not using res knot for a linter. That goal remains unchanged 

---

_@MichaReiser reviewed on 2024-08-16 20:18_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:145 on 2024-08-16 20:18_

I think the difference here is that for parameters this is actually useful. For example: give me the symbol of the parameter and what is its declared type is an operation that seems useful to me. I definitely don't want partial HasTy implementations that sometimes panic 

---

_Comment by @MichaReiser on 2024-08-16 20:32_

> Thanks for updating this test! I think it's really valuable to validate our expression coverage in type inference.
> 
> 
> 
> Sorry for the wall-of-text in some of the comments; this PR raises a lot of subtle questions!

Thanks for the review! It does make sense to me to be more selective when it comes to implementation HasTy for definitions. I'll remove the problematic ones for now (all assignments). I rather have this merged and us improve it incrementally than pilling a lot unrelated fixes into it (that all raise subtle design decay)

---

_@carljm reviewed on 2024-08-16 20:42_

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:61 on 2024-08-16 20:42_

Yes, definitely agree long-term goal hasn't changed, I should have been clearer that the question was just about short-term future/maintenance.

But LSP is a great point, and answers that question as well!

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:145 on 2024-08-16 20:46_

I think one way to make this not panic  might be to do scope-wide type inference (and make sure that sets types on all parameter nodes), and fallback to that route for parameter nodes that don't have an entry in the node-to-definitions map in semantic index. (EDIT: never mind, I don't think we can do this one, since Parameter nodes aren't expressions and thus don't have a `ScopedExpressionId` and can't go in the expression-types map.)

It might be possible to handle this by putting two entries into the node-to-definitions map (one for the `ParameterWithDefault` and another for the nested `Parameter`) that both map to the same Definition. This would effectively give us a very localized way to "walk up" that one level of the AST. Not sure if there'd be any downsides to having two nodes map to the same Definition in that map.

---

_@carljm reviewed on 2024-08-16 20:46_

---

_@MichaReiser reviewed on 2024-08-17 11:39_

I removed the `HasTy` implementation for `Comprehension`. I think it has the same flaws as `Assignment` where it isn't clear if it is the target's type or the type of the value (or what not)

---

_@MichaReiser reviewed on 2024-08-17 11:51_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:145 on 2024-08-17 11:51_

That's a great suggestion. I implemented the definition redirect.

---

_Merged by @MichaReiser on 2024-08-17 11:59_

---

_Closed by @MichaReiser on 2024-08-17 11:59_

---

_Branch deleted on 2024-08-17 11:59_

---
