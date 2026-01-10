```yaml
number: 20880
title: "[`flake8-todos`] Extend TD003 to allow Jira-style issue IDs on the same line (`TD003`)"
type: pull_request
state: open
author: danparizher
labels:
  - rule
assignees: []
base: main
head: fix-20809
created_at: 2025-10-15T03:31:44Z
updated_at: 2025-10-27T09:25:38Z
url: https://github.com/astral-sh/ruff/pull/20880
synced_at: 2026-01-10T16:59:49Z
```

# [`flake8-todos`] Extend TD003 to allow Jira-style issue IDs on the same line (`TD003`)

---

_Pull request opened by @danparizher on 2025-10-15 03:31_

## Summary

Fixes #20809. Tightens regex pattern and adds test coverage based on ecosystem analysis.

## Ecosystem Analysis

**51 TD003 violations removed** from real-world projects:

- Apache Airflow: 50 violations
- Apache Superset: 1 violation

These represent legitimate Jira-style issue IDs previously flagged as missing links.

## Key Changes

### Pattern Specificity Fix

- **Before**: `[A-Z]+-\d+` (accepted single-letter keys like `A-1`)
- **After**: `[A-Z]{2,}-\d+` (requires minimum 2 uppercase letters)
- **Rationale**: Aligns with Jira project key requirements (2-10 uppercase letters)

### Test Coverage

Added test cases:

- ✅ Valid: `ABC-123`, `PROJ-456`, `RFFU-6877`
- ❌ Invalid: `A-1`, `abc-123`, `ABC123`, `123-ABC`

---

_Comment by @github-actions[bot] on 2025-10-15 03:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -37 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/cli/commands/task_command.py#L447'>airflow-core/src/airflow/cli/commands/task_command.py:447:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/dag_processing/dagbag.py#L206'>airflow-core/src/airflow/dag_processing/dagbag.py:206:50:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L112'>airflow-core/src/airflow/models/mappedoperator.py:112:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L160'>airflow-core/src/airflow/models/mappedoperator.py:160:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L420'>airflow-core/src/airflow/models/mappedoperator.py:420:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L425'>airflow-core/src/airflow/models/mappedoperator.py:425:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L438'>airflow-core/src/airflow/models/mappedoperator.py:438:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L447'>airflow-core/src/airflow/models/mappedoperator.py:447:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L456'>airflow-core/src/airflow/models/mappedoperator.py:456:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L482'>airflow-core/src/airflow/models/mappedoperator.py:482:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L490'>airflow-core/src/airflow/models/mappedoperator.py:490:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L501'>airflow-core/src/airflow/models/mappedoperator.py:501:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L512'>airflow-core/src/airflow/models/mappedoperator.py:512:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L519'>airflow-core/src/airflow/models/mappedoperator.py:519:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L563'>airflow-core/src/airflow/models/mappedoperator.py:563:19:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L85'>airflow-core/src/airflow/models/mappedoperator.py:85:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/taskinstance.py#L1622'>airflow-core/src/airflow/models/taskinstance.py:1622:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/taskinstance.py#L1763'>airflow-core/src/airflow/models/taskinstance.py:1763:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/definitions/notset.py#L23'>airflow-core/src/airflow/serialization/definitions/notset.py:23:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/definitions/taskgroup.py#L104'>airflow-core/src/airflow/serialization/definitions/taskgroup.py:104:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/definitions/taskgroup.py#L48'>airflow-core/src/airflow/serialization/definitions/taskgroup.py:48:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L1220'>airflow-core/src/airflow/serialization/serialized_objects.py:1220:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L1248'>airflow-core/src/airflow/serialization/serialized_objects.py:1248:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L1352'>airflow-core/src/airflow/serialization/serialized_objects.py:1352:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2243'>airflow-core/src/airflow/serialization/serialized_objects.py:2243:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2256'>airflow-core/src/airflow/serialization/serialized_objects.py:2256:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2265'>airflow-core/src/airflow/serialization/serialized_objects.py:2265:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2274'>airflow-core/src/airflow/serialization/serialized_objects.py:2274:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L685'>airflow-core/src/airflow/serialization/serialized_objects.py:685:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/utils/dag_edges.py#L115'>airflow-core/src/airflow/utils/dag_edges.py:115:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/utils/dag_edges.py#L121'>airflow-core/src/airflow/utils/dag_edges.py:121:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/always/test_example_dags.py#L130'>airflow-core/tests/unit/always/test_example_dags.py:130:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/serialization/test_dag_serialization.py#L371'>airflow-core/tests/unit/serialization/test_dag_serialization.py:371:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/utils/test_db_cleanup.py#L446'>airflow-core/tests/unit/utils/test_db_cleanup.py:446:28:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/utils/test_db_cleanup.py#L447'>airflow-core/tests/unit/utils/test_db_cleanup.py:447:36:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/task-sdk/src/airflow/sdk/definitions/dag.py#L518'>task-sdk/src/airflow/sdk/definitions/dag.py:518:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/task-sdk/src/airflow/sdk/definitions/mappedoperator.py#L786'>task-sdk/src/airflow/sdk/definitions/mappedoperator.py:786:7:</a> TD003 Missing issue link for this TODO
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TD003 | 37 | 0 | 37 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -37 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/cli/commands/task_command.py#L447'>airflow-core/src/airflow/cli/commands/task_command.py:447:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/dag_processing/dagbag.py#L206'>airflow-core/src/airflow/dag_processing/dagbag.py:206:50:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L112'>airflow-core/src/airflow/models/mappedoperator.py:112:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L160'>airflow-core/src/airflow/models/mappedoperator.py:160:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L420'>airflow-core/src/airflow/models/mappedoperator.py:420:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L425'>airflow-core/src/airflow/models/mappedoperator.py:425:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L438'>airflow-core/src/airflow/models/mappedoperator.py:438:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L447'>airflow-core/src/airflow/models/mappedoperator.py:447:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L456'>airflow-core/src/airflow/models/mappedoperator.py:456:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L482'>airflow-core/src/airflow/models/mappedoperator.py:482:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L490'>airflow-core/src/airflow/models/mappedoperator.py:490:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L501'>airflow-core/src/airflow/models/mappedoperator.py:501:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L512'>airflow-core/src/airflow/models/mappedoperator.py:512:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L519'>airflow-core/src/airflow/models/mappedoperator.py:519:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L563'>airflow-core/src/airflow/models/mappedoperator.py:563:19:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/mappedoperator.py#L85'>airflow-core/src/airflow/models/mappedoperator.py:85:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/taskinstance.py#L1622'>airflow-core/src/airflow/models/taskinstance.py:1622:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/models/taskinstance.py#L1763'>airflow-core/src/airflow/models/taskinstance.py:1763:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/definitions/notset.py#L23'>airflow-core/src/airflow/serialization/definitions/notset.py:23:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/definitions/taskgroup.py#L104'>airflow-core/src/airflow/serialization/definitions/taskgroup.py:104:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/definitions/taskgroup.py#L48'>airflow-core/src/airflow/serialization/definitions/taskgroup.py:48:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L1220'>airflow-core/src/airflow/serialization/serialized_objects.py:1220:3:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L1248'>airflow-core/src/airflow/serialization/serialized_objects.py:1248:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L1352'>airflow-core/src/airflow/serialization/serialized_objects.py:1352:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2243'>airflow-core/src/airflow/serialization/serialized_objects.py:2243:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2256'>airflow-core/src/airflow/serialization/serialized_objects.py:2256:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2265'>airflow-core/src/airflow/serialization/serialized_objects.py:2265:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L2274'>airflow-core/src/airflow/serialization/serialized_objects.py:2274:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/serialization/serialized_objects.py#L685'>airflow-core/src/airflow/serialization/serialized_objects.py:685:11:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/utils/dag_edges.py#L115'>airflow-core/src/airflow/utils/dag_edges.py:115:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/src/airflow/utils/dag_edges.py#L121'>airflow-core/src/airflow/utils/dag_edges.py:121:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/always/test_example_dags.py#L130'>airflow-core/tests/unit/always/test_example_dags.py:130:15:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/serialization/test_dag_serialization.py#L371'>airflow-core/tests/unit/serialization/test_dag_serialization.py:371:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/utils/test_db_cleanup.py#L446'>airflow-core/tests/unit/utils/test_db_cleanup.py:446:28:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/airflow-core/tests/unit/utils/test_db_cleanup.py#L447'>airflow-core/tests/unit/utils/test_db_cleanup.py:447:36:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/task-sdk/src/airflow/sdk/definitions/dag.py#L518'>task-sdk/src/airflow/sdk/definitions/dag.py:518:7:</a> TD003 Missing issue link for this TODO
- <a href='https://github.com/apache/airflow/blob/970d7dac23a46fd44c2bb0da81606dadd9fda402/task-sdk/src/airflow/sdk/definitions/mappedoperator.py#L786'>task-sdk/src/airflow/sdk/definitions/mappedoperator.py:786:7:</a> TD003 Missing issue link for this TODO
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TD003 | 37 | 0 | 37 | 0 | 0 |

</p>
</details>




---

_Review requested from @dylwil3 by @ntBre on 2025-10-15 14:20_

---

_Comment by @MichaReiser on 2025-10-17 07:50_

Could you please take a look at the ecosystem changes and summarize your findings?

---

_Comment by @MichaReiser on 2025-10-20 07:03_

- [superset/tasks/async_queries.py:107:15:](https://github.com/apache/superset/blob/0b3fe3d60ccbb5d80370af3baafcd74879d34de6/superset/tasks/async_queries.py#L107) TD003 Missing issue link for this TODO

This is now a false negative

---

_@MichaReiser reviewed on 2025-10-20 07:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:251 on 2025-10-20 07:04_

Looking through the ecosystem reports, I think we should make this more strict. E.g. any of:

* Comes directly after todo (`TODO RFFU-6887` or `TODO: RFFU-6887)
* In parentheses `(RFFU-6877)`
* At the end of the line


@simonpercivall how do you format your Jira style issue codes that are easy to detect but don't lead to accidential false-negatives?


---

_Review comment by @simonpercivall on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:251 on 2025-10-20 07:23_

Personally wouldn't be unhappy with requiring parentheses if at the end of the line.

We generally have just put it as the last item on the line, but with no (or arbitrary) separation. Consistency via a rule would just be helpful.

---

_@simonpercivall reviewed on 2025-10-20 07:23_

---

_Label `rule` added by @MichaReiser on 2025-10-21 07:30_

---

_@MichaReiser reviewed on 2025-10-21 07:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:252 on 2025-10-21 07:33_

This one still seems to permissive and makes the other regex patterns mostly redundant (and we still see the false positives). Can we limit the pattern to only be allowed right after the TODO or if followed by a `:`?

---

_@MichaReiser reviewed on 2025-10-24 06:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:251 on 2025-10-24 06:26_

@danparizher can you explain the your reasoning for why you only allow the codes at the end of a line, in parentheses, or after a colon but e.g. not when it's directly after a `TODO`?

---

_@danparizher reviewed on 2025-10-24 21:54_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:251 on 2025-10-24 21:54_

@MichaReiser The regex patterns are intentionally restrictive to prevent false positives. If we matched issue codes directly after "TODO" (like `# TODO PROJ-123`), we'd incorrectly flag legitimate descriptions that happen to mention issue numbers, such as `# TODO: Fix bug ABC-123` or `# TODO: Working on PROJ-123 and PROJ-456`.

By requiring issue codes the way they are currently implemented, we ensure they're deliberately placed as references rather than accidentally mentioned in descriptive text. This follows the principle that it's better to miss some edge cases than to generate incorrect warnings.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:104 on 2025-10-27 09:18_

Looking at the tests, won't this still trigger a violation as it is the same as 

```
# TODO: PROJ-123 Another Jira-style example
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:251 on 2025-10-27 09:25_

I'm not sure how a code like `TODO PROJ-123` or `TODO: PROJ-123` would be an issue here. 

At the same time, I start to worry that explaining the behavior of this rule is becoming increasingly difficult.

@simonpercivall are there any other tools we could take inspiration from how to handle JIRA comments. E.g. does VS Code (or is there an extension) that allows you to click on JIRA issue codes? Do you know what formats it detects?

---

_@MichaReiser reviewed on 2025-10-27 09:25_

---
