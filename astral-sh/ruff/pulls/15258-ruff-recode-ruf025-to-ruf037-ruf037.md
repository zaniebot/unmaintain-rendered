```yaml
number: 15258
title: "[`ruff`] Recode `RUF025` to `RUF037` (`RUF037`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: main
head: RUF037
created_at: 2025-01-04T15:32:23Z
updated_at: 2025-01-05T08:49:05Z
url: https://github.com/astral-sh/ruff/pull/15258
synced_at: 2026-01-12T15:55:50Z
```

# [`ruff`] Recode `RUF025` to `RUF037` (`RUF037`)

---

_@InSyncWithFoo_

## Summary

Resolves #15256.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-04 15:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/training/test_interactive.py#L663'>tests/core/training/test_interactive.py:663:58:</a> RUF025 [*] Unnecessary empty iterable within a deque call
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/training/test_interactive.py#L663'>tests/core/training/test_interactive.py:663:58:</a> RUF037 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c37d5cf71d0c426371cdf059b5b2bc11a103c5ba/tests/jobs/test_scheduler_job.py#L5887'>tests/jobs/test_scheduler_job.py:5887:38:</a> RUF025 [*] Unnecessary empty iterable within a deque call
+ <a href='https://github.com/apache/airflow/blob/c37d5cf71d0c426371cdf059b5b2bc11a103c5ba/tests/jobs/test_scheduler_job.py#L5887'>tests/jobs/test_scheduler_job.py:5887:38:</a> RUF037 [*] Unnecessary empty iterable within a deque call
- <a href='https://github.com/apache/airflow/blob/c37d5cf71d0c426371cdf059b5b2bc11a103c5ba/tests/jobs/test_scheduler_job.py#L5888'>tests/jobs/test_scheduler_job.py:5888:43:</a> RUF025 [*] Unnecessary empty iterable within a deque call
+ <a href='https://github.com/apache/airflow/blob/c37d5cf71d0c426371cdf059b5b2bc11a103c5ba/tests/jobs/test_scheduler_job.py#L5888'>tests/jobs/test_scheduler_job.py:5888:43:</a> RUF037 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF037 | 3 | 3 | 0 | 0 | 0 |
| RUF025 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---

_@MichaReiser approved on 2025-01-05 08:35_

Thank you

---

_Merged by @MichaReiser on 2025-01-05 08:35_

---

_Closed by @MichaReiser on 2025-01-05 08:35_

---

_Branch deleted on 2025-01-05 08:49_

---
