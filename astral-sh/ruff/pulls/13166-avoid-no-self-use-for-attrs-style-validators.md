```yaml
number: 13166
title: "Avoid `no-self-use` for `attrs`-style validators"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/conflict
created_at: 2024-08-30T16:31:18Z
updated_at: 2024-08-30T16:45:50Z
url: https://github.com/astral-sh/ruff/pull/13166
synced_at: 2026-01-10T21:38:32Z
```

# Avoid `no-self-use` for `attrs`-style validators

---

_Pull request opened by @charliermarsh on 2024-08-30 16:31_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12568.


---

_Label `rule` added by @charliermarsh on 2024-08-30 16:31_

---

_Merged by @charliermarsh on 2024-08-30 16:39_

---

_Closed by @charliermarsh on 2024-08-30 16:39_

---

_Branch deleted on 2024-08-30 16:39_

---

_Comment by @github-actions[bot] on 2024-08-30 16:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8b0a78120c1097fed363cd45f5297801e8e73860/airflow/decorators/task_group.py#L71'>airflow/decorators/task_group.py:71:9:</a> PLR6301 Method `_validate` could be a function, class method, or static method
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-08-30 16:45_

Ecosystem change is correct!

---
