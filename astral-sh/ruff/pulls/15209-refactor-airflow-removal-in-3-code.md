```yaml
number: 15209
title: Refactor Airflow removal in 3 code
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/airflow-refactor
created_at: 2024-12-31T05:16:04Z
updated_at: 2024-12-31T05:28:35Z
url: https://github.com/astral-sh/ruff/pull/15209
synced_at: 2026-01-10T20:42:27Z
```

# Refactor Airflow removal in 3 code

---

_Pull request opened by @dhruvmanila on 2024-12-31 05:16_

This PR contains a couple of refactors for the Airflow removal in 3 code, nothing major just some minor nits.

---

_Label `internal` added by @dhruvmanila on 2024-12-31 05:16_

---

_Comment by @github-actions[bot] on 2024-12-31 05:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/fab/auth_manager/test_fab_auth_manager.py#L87'>providers/tests/fab/auth_manager/test_fab_auth_manager.py:87:26:</a> AIR302 `appbuilder` is removed in Airflow 3.0; The constructor takes no parameter now
- <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/tests/fab/auth_manager/test_fab_auth_manager.py#L87'>providers/tests/fab/auth_manager/test_fab_auth_manager.py:87:26:</a> AIR302 `appbuilder` is removed in Airflow 3.0; The constructor takes no parameter now.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @dhruvmanila on 2024-12-31 05:27_

---

_Closed by @dhruvmanila on 2024-12-31 05:27_

---

_Branch deleted on 2024-12-31 05:27_

---
