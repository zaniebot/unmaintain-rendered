```yaml
number: 10464
title: Avoid code comment detection in PEP 723 script tags
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/script
created_at: 2024-03-18T21:33:15Z
updated_at: 2024-03-18T21:48:52Z
url: https://github.com/astral-sh/ruff/pull/10464
synced_at: 2026-01-12T15:55:32Z
```

# Avoid code comment detection in PEP 723 script tags

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/10455.

---

_Label `bug` added by @charliermarsh on 2024-03-18 21:33_

---

_Marked ready for review by @charliermarsh on 2024-03-18 21:33_

---

_Comment by @github-actions[bot] on 2024-03-18 21:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/29ac05f4969f54815c82d6af9211798aa53c45c3/clients/python/test_python_client.py#L21'>clients/python/test_python_client.py:21:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/29ac05f4969f54815c82d6af9211798aa53c45c3/clients/python/test_python_client.py#L24'>clients/python/test_python_client.py:24:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ERA001 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/29ac05f4969f54815c82d6af9211798aa53c45c3/clients/python/test_python_client.py#L21'>clients/python/test_python_client.py:21:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/29ac05f4969f54815c82d6af9211798aa53c45c3/clients/python/test_python_client.py#L24'>clients/python/test_python_client.py:24:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ERA001 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-03-18 21:48_

Ecosystem change is a real false positive.

---

_Merged by @charliermarsh on 2024-03-18 21:48_

---

_Closed by @charliermarsh on 2024-03-18 21:48_

---

_Branch deleted on 2024-03-18 21:48_

---
