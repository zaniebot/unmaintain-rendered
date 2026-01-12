```yaml
number: 9144
title: improve error message for RUF006
type: pull_request
state: closed
author: danking
labels: []
assignees: []
base: main
head: patch-1
created_at: 2023-12-15T03:21:19Z
updated_at: 2023-12-15T03:33:33Z
url: https://github.com/astral-sh/ruff/pull/9144
synced_at: 2026-01-12T15:55:27Z
```

# improve error message for RUF006

---

_@danking_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #9133

Ruff cannot always prove a task is awaited. The error message should be clear about this possibility.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

The tests were updated with the new error message.

<!-- How was it tested? -->


---

_Closed by @danking on 2023-12-15 03:30_

---

_Comment by @github-actions[bot] on 2023-12-15 03:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/01fd0d31b46682f4d700aaacf19cfe7a0fe9a057/tests/providers/google/cloud/triggers/test_bigquery.py#L601'>tests/providers/google/cloud/triggers/test_bigquery.py:601:9:</a> RUF006 Cannot prove result of `asyncio.create_task` is awaited in all program traces.
- <a href='https://github.com/apache/airflow/blob/01fd0d31b46682f4d700aaacf19cfe7a0fe9a057/tests/providers/google/cloud/triggers/test_bigquery.py#L601'>tests/providers/google/cloud/triggers/test_bigquery.py:601:9:</a> RUF006 Store a reference to the return value of `asyncio.create_task`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF006 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/01fd0d31b46682f4d700aaacf19cfe7a0fe9a057/tests/providers/google/cloud/triggers/test_bigquery.py#L601'>tests/providers/google/cloud/triggers/test_bigquery.py:601:9:</a> RUF006 Cannot prove result of `asyncio.create_task` is awaited in all program traces.
- <a href='https://github.com/apache/airflow/blob/01fd0d31b46682f4d700aaacf19cfe7a0fe9a057/tests/providers/google/cloud/triggers/test_bigquery.py#L601'>tests/providers/google/cloud/triggers/test_bigquery.py:601:9:</a> RUF006 Store a reference to the return value of `asyncio.create_task`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF006 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---
