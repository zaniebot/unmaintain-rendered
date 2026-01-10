```yaml
number: 12045
title: "[Ruff 0.5] Stabilise `manual-dict-comprehension` (`PERF403`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: perflint-stabilisation
created_at: 2024-06-26T11:16:29Z
updated_at: 2024-06-26T12:01:41Z
url: https://github.com/astral-sh/ruff/pull/12045
synced_at: 2026-01-10T21:56:00Z
```

# [Ruff 0.5] Stabilise `manual-dict-comprehension` (`PERF403`)

---

_Pull request opened by @AlexWaygood on 2024-06-26 11:16_

## Summary

This rule's been in preview for a very long time (over 9 months, if I'm not mistaken). There are no open issues about it, the docs accurately describe what it does, and the implementation seems sound.

It's an opinionated rule that advocates a micro-optimisation -- but this is true for all the perflint rules, and is flagged in the docs. I don't think you'd enable the `PERF` category at all if you weren't interested in this kind of rule.


---

_Label `rule` added by @AlexWaygood on 2024-06-26 11:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-26 11:16_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-06-26 11:16_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-06-26 11:16_

---

_Comment by @github-actions[bot] on 2024-06-26 11:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L149'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:149:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/exasol/hooks/exasol.py#L68'>airflow/providers/exasol/hooks/exasol.py:68:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/mysql/hooks/mysql.py#L171'>airflow/providers/mysql/hooks/mysql.py:171:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/postgres/hooks/postgres.py#L173'>airflow/providers/postgres/hooks/postgres.py:173:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2486'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2486:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/tests/providers/apache/spark/hooks/test_spark_submit.py#L74'>tests/providers/apache/spark/hooks/test_spark_submit.py:74:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF403 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-06-26 11:41_

The ecosystem report LGTM. Those indeed could all be replaced with dict comprehensions if you wanted to micro-optimise your code.

---

_@dhruvmanila approved on 2024-06-26 12:00_

I think all ecosystem changes except for the last one would require using `dict.update` method. This part is documented in the [example section], so seems good to promote to stable.

---

_@charliermarsh approved on 2024-06-26 12:01_

---

_Merged by @AlexWaygood on 2024-06-26 12:01_

---

_Closed by @AlexWaygood on 2024-06-26 12:01_

---

_Branch deleted on 2024-06-26 12:01_

---
