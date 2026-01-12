```yaml
number: 8072
title: "Avoid if-else simplification for `TYPE_CHECKING` blocks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/SIM108
created_at: 2023-10-19T18:52:06Z
updated_at: 2023-10-19T23:15:55Z
url: https://github.com/astral-sh/ruff/pull/8072
synced_at: 2026-01-12T02:32:41Z
```

# Avoid if-else simplification for `TYPE_CHECKING` blocks

---

_Pull request opened by @charliermarsh on 2023-10-19 18:52_

Closes https://github.com/astral-sh/ruff/issues/8071.

---

_Label `bug` added by @charliermarsh on 2023-10-19 18:52_

---

_Review requested from @zanieb by @charliermarsh on 2023-10-19 18:53_

---

_Comment by @charliermarsh on 2023-10-19 19:03_

We can merge this, but I should audit the other `if` rules.

---

_Comment by @github-actions[bot] on 2023-10-19 19:10_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8d0ad02ded80910decd41c7041dc0753083fb774/airflow/models/xcom.py#L875'>airflow/models/xcom.py:875:1:</a> SIM108 Use ternary operator `XCom = BaseXCom if TYPE_CHECKING else resolve_xcom_backend()` instead of `if`-`else`-block
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM108 | 1 | 0 | 1 |



---

_Merged by @charliermarsh on 2023-10-19 23:15_

---

_Closed by @charliermarsh on 2023-10-19 23:15_

---

_Branch deleted on 2023-10-19 23:15_

---
