```yaml
number: 11287
title: "Allow `set(True)` for boolean traps"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/fbt
created_at: 2024-05-04T21:26:11Z
updated_at: 2024-05-04T21:39:16Z
url: https://github.com/astral-sh/ruff/pull/11287
synced_at: 2026-01-10T22:37:02Z
```

# Allow `set(True)` for boolean traps

---

_Pull request opened by @charliermarsh on 2024-05-04 21:26_

Closes https://github.com/astral-sh/ruff/issues/8923.

---

_Label `rule` added by @charliermarsh on 2024-05-04 21:26_

---

_Merged by @charliermarsh on 2024-05-04 21:33_

---

_Closed by @charliermarsh on 2024-05-04 21:33_

---

_Branch deleted on 2024-05-04 21:33_

---

_Comment by @github-actions[bot] on 2024-05-04 21:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1d234aa2ab8b300055bb3ec51bdd1168651b7a47/airflow/jobs/triggerer_job_runner.py#L596'>airflow/jobs/triggerer_job_runner.py:596:29:</a> FBT003 Boolean positional value in function call
- <a href='https://github.com/apache/airflow/blob/1d234aa2ab8b300055bb3ec51bdd1168651b7a47/airflow/jobs/triggerer_job_runner.py#L599'>airflow/jobs/triggerer_job_runner.py:599:31:</a> FBT003 Boolean positional value in function call
- <a href='https://github.com/apache/airflow/blob/1d234aa2ab8b300055bb3ec51bdd1168651b7a47/airflow/jobs/triggerer_job_runner.py#L635'>airflow/jobs/triggerer_job_runner.py:635:29:</a> FBT003 Boolean positional value in function call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT003 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1d234aa2ab8b300055bb3ec51bdd1168651b7a47/airflow/jobs/triggerer_job_runner.py#L596'>airflow/jobs/triggerer_job_runner.py:596:29:</a> FBT003 Boolean positional value in function call
- <a href='https://github.com/apache/airflow/blob/1d234aa2ab8b300055bb3ec51bdd1168651b7a47/airflow/jobs/triggerer_job_runner.py#L599'>airflow/jobs/triggerer_job_runner.py:599:31:</a> FBT003 Boolean positional value in function call
- <a href='https://github.com/apache/airflow/blob/1d234aa2ab8b300055bb3ec51bdd1168651b7a47/airflow/jobs/triggerer_job_runner.py#L635'>airflow/jobs/triggerer_job_runner.py:635:29:</a> FBT003 Boolean positional value in function call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT003 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---
