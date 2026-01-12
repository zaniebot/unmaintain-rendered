```yaml
number: 9969
title: Respect tuple assignments in typing analyzer
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2024-02-13T04:48:13Z
updated_at: 2024-02-13T05:08:46Z
url: https://github.com/astral-sh/ruff/pull/9969
synced_at: 2026-01-12T15:55:30Z
```

# Respect tuple assignments in typing analyzer

---

_@charliermarsh_

## Summary

Just addressing some discrepancies between the analyzers like `is_dict` and the logic that's matured in `find_binding_value`.

---

_Label `bug` added by @charliermarsh on 2024-02-13 04:48_

---

_Marked ready for review by @charliermarsh on 2024-02-13 04:48_

---

_Merged by @charliermarsh on 2024-02-13 05:02_

---

_Closed by @charliermarsh on 2024-02-13 05:02_

---

_Branch deleted on 2024-02-13 05:02_

---

_Comment by @github-actions[bot] on 2024-02-13 05:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b48280c0530de1dafd9fcf428f3ce6264fcbdc97/airflow/providers/google/cloud/operators/bigquery.py#L186'>airflow/providers/google/cloud/operators/bigquery.py:186:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF403 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---
