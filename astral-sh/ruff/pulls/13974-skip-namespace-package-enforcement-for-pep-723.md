```yaml
number: 13974
title: Skip namespace package enforcement for PEP 723 scripts
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/pep723
created_at: 2024-10-29T02:05:54Z
updated_at: 2024-10-29T02:26:08Z
url: https://github.com/astral-sh/ruff/pull/13974
synced_at: 2026-01-12T15:55:46Z
```

# Skip namespace package enforcement for PEP 723 scripts

---

_@charliermarsh_

## Summary

Vendors the PEP 723 parser from [uv](https://github.com/astral-sh/uv/blob/debe67ffdb0cd7835734100e909b2d8f79613743/crates/uv-scripts/src/lib.rs#L283).

Closes https://github.com/astral-sh/ruff/issues/13912.


---

_Label `rule` added by @charliermarsh on 2024-10-29 02:06_

---

_Merged by @charliermarsh on 2024-10-29 02:11_

---

_Closed by @charliermarsh on 2024-10-29 02:11_

---

_Branch deleted on 2024-10-29 02:11_

---

_Comment by @github-actions[bot] on 2024-10-29 02:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c2e4d21742c184d2d041fc93b2dd8ee9fede9cb5/clients/python/test_python_client.py#L1'>clients/python/test_python_client.py:1:1:</a> INP001 File `clients/python/test_python_client.py` is part of an implicit namespace package. Add an `__init__.py`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c2e4d21742c184d2d041fc93b2dd8ee9fede9cb5/clients/python/test_python_client.py#L1'>clients/python/test_python_client.py:1:1:</a> INP001 File `clients/python/test_python_client.py` is part of an implicit namespace package. Add an `__init__.py`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
