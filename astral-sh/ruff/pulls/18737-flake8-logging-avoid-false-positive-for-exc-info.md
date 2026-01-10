```yaml
number: 18737
title: "[`flake8-logging`] Avoid false positive for `exc_info=True` outside `logger.exception` (`LOG014`)"
type: pull_request
state: merged
author: hmvp
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix_log014
created_at: 2025-06-17T20:58:53Z
updated_at: 2025-06-20T18:43:08Z
url: https://github.com/astral-sh/ruff/pull/18737
synced_at: 2026-01-10T18:39:08Z
```

# [`flake8-logging`] Avoid false positive for `exc_info=True` outside `logger.exception` (`LOG014`)

---

_Pull request opened by @hmvp on 2025-06-17 20:58_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ruff/issues/18726 by also checking if its a literal and not only that it is truthy. See also the first comment in the issue.

It would have been nice to check for inheritance of BaseException but I figured that is not possible yet...

## Test Plan

I added a few tests for valid input to exc_info


---

_Label `bug` added by @ntBre on 2025-06-17 22:45_

---

_Label `rule` added by @ntBre on 2025-06-17 22:45_

---

_Review requested from @ntBre by @ntBre on 2025-06-17 22:45_

---

_Comment by @github-actions[bot] on 2025-06-17 22:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7cecc66069a2588dfaba95a030f09076552c4c68/airflow-core/src/airflow/api_fastapi/execution_api/app.py#L187'>airflow-core/src/airflow/api_fastapi/execution_api/app.py:187:55:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG014 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7cecc66069a2588dfaba95a030f09076552c4c68/airflow-core/src/airflow/api_fastapi/execution_api/app.py#L187'>airflow-core/src/airflow/api_fastapi/execution_api/app.py:187:55:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG014 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_@ntBre approved on 2025-06-20 18:42_

Looks great, thank you!

---

_Merged by @ntBre on 2025-06-20 18:43_

---

_Closed by @ntBre on 2025-06-20 18:43_

---
