```yaml
number: 10206
title: "Avoid false-positives for parens-on-raise with `future.exception()`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/future
created_at: 2024-03-03T00:20:25Z
updated_at: 2024-03-06T19:53:58Z
url: https://github.com/astral-sh/ruff/pull/10206
synced_at: 2026-01-12T15:55:31Z
```

# Avoid false-positives for parens-on-raise with `future.exception()`

---

_@charliermarsh_

## Summary

As a heuristic, we now ignore function calls that "look like" method calls (e.g., `future.exception()`).

Closes https://github.com/astral-sh/ruff/issues/10205.


---

_Label `bug` added by @charliermarsh on 2024-03-03 00:22_

---

_Renamed from "Avoid false-positives for parens-on-raise with futures.exception()" to "Avoid false-positives for parens-on-raise with `futures.exception()`" by @charliermarsh on 2024-03-03 00:22_

---

_Merged by @charliermarsh on 2024-03-03 00:28_

---

_Closed by @charliermarsh on 2024-03-03 00:28_

---

_Branch deleted on 2024-03-03 00:28_

---

_Comment by @github-actions[bot] on 2024-03-03 00:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/73a632a5a0af3fe51484e89a7bd4c771800707e5/airflow/exceptions.py#L238'>airflow/exceptions.py:238:22:</a> RSE102 Unnecessary parentheses on raised exception
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RSE102 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/73a632a5a0af3fe51484e89a7bd4c771800707e5/airflow/exceptions.py#L238'>airflow/exceptions.py:238:22:</a> RSE102 Unnecessary parentheses on raised exception
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RSE102 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Renamed from "Avoid false-positives for parens-on-raise with `futures.exception()`" to "Avoid false-positives for parens-on-raise with `future.exception()`" by @AlexWaygood on 2024-03-06 19:53_

---
