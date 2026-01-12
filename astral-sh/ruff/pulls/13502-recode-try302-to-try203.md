```yaml
number: 13502
title: "Recode `TRY302` to `TRY203`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: ruff-0.7
head: zb/try203
created_at: 2024-09-24T13:56:15Z
updated_at: 2024-10-08T16:41:29Z
url: https://github.com/astral-sh/ruff/pull/13502
synced_at: 2026-01-12T15:55:44Z
```

# Recode `TRY302` to `TRY203`

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/13492


---

_Added to milestone `v0.7` by @zanieb on 2024-09-24 13:56_

---

_Comment by @github-actions[bot] on 2024-09-24 14:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/hooks/kubernetes.py#L493'>airflow/providers/cncf/kubernetes/hooks/kubernetes.py:493:9:</a> TRY203 Remove exception handler; error is immediately re-raised
- <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/hooks/kubernetes.py#L493'>airflow/providers/cncf/kubernetes/hooks/kubernetes.py:493:9:</a> TRY302 Remove exception handler; error is immediately re-raised
+ <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/operators/pod.py#L802'>airflow/providers/cncf/kubernetes/operators/pod.py:802:9:</a> TRY203 Remove exception handler; error is immediately re-raised
- <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/operators/pod.py#L802'>airflow/providers/cncf/kubernetes/operators/pod.py:802:9:</a> TRY302 Remove exception handler; error is immediately re-raised
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRY203 | 2 | 2 | 0 | 0 | 0 |
| TRY302 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/hooks/kubernetes.py#L493'>airflow/providers/cncf/kubernetes/hooks/kubernetes.py:493:9:</a> TRY203 Remove exception handler; error is immediately re-raised
- <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/hooks/kubernetes.py#L493'>airflow/providers/cncf/kubernetes/hooks/kubernetes.py:493:9:</a> TRY302 Remove exception handler; error is immediately re-raised
+ <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/operators/pod.py#L802'>airflow/providers/cncf/kubernetes/operators/pod.py:802:9:</a> TRY203 Remove exception handler; error is immediately re-raised
- <a href='https://github.com/apache/airflow/blob/e14b4cae9c61cbf1df6e6b573d926d614f9f63e5/airflow/providers/cncf/kubernetes/operators/pod.py#L802'>airflow/providers/cncf/kubernetes/operators/pod.py:802:9:</a> TRY302 Remove exception handler; error is immediately re-raised
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRY203 | 2 | 2 | 0 | 0 | 0 |
| TRY302 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @zanieb on 2024-10-08 15:29_

---

_@AlexWaygood approved on 2024-10-08 16:41_

---

_Merged by @AlexWaygood on 2024-10-08 16:41_

---

_Closed by @AlexWaygood on 2024-10-08 16:41_

---

_Branch deleted on 2024-10-08 16:41_

---
