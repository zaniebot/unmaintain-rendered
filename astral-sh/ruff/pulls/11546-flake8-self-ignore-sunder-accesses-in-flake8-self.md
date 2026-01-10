```yaml
number: 11546
title: "[`flake8-self`] Ignore sunder accesses in `flake8-self` rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-05-26T17:41:16Z
updated_at: 2024-05-26T17:57:25Z
url: https://github.com/astral-sh/ruff/pull/11546
synced_at: 2026-01-10T22:05:27Z
```

# [`flake8-self`] Ignore sunder accesses in `flake8-self` rule

---

_Pull request opened by @charliermarsh on 2024-05-26 17:41_

## Summary

We already ignore dunders, so ignoring sunders (as in https://docs.python.org/3/library/enum.html#supported-sunder-names) makes sense to me.

---

_Label `rule` added by @charliermarsh on 2024-05-26 17:41_

---

_Comment by @github-actions[bot] on 2024-05-26 17:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L201'>tests/providers/elasticsearch/log/test_es_response.py:201:16:</a> SLF001 Private member accessed: `_d_`
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L53'>tests/providers/elasticsearch/log/test_es_response.py:53:16:</a> SLF001 Private member accessed: `_l_`
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L57'>tests/providers/elasticsearch/log/test_es_response.py:57:16:</a> SLF001 Private member accessed: `_l_`
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L87'>tests/providers/elasticsearch/log/test_es_response.py:87:16:</a> SLF001 Private member accessed: `_d_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SLF001 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L201'>tests/providers/elasticsearch/log/test_es_response.py:201:16:</a> SLF001 Private member accessed: `_d_`
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L53'>tests/providers/elasticsearch/log/test_es_response.py:53:16:</a> SLF001 Private member accessed: `_l_`
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L57'>tests/providers/elasticsearch/log/test_es_response.py:57:16:</a> SLF001 Private member accessed: `_l_`
- <a href='https://github.com/apache/airflow/blob/993053ad3ea5a0d0dea7f3814d2497115392a620/tests/providers/elasticsearch/log/test_es_response.py#L87'>tests/providers/elasticsearch/log/test_es_response.py:87:16:</a> SLF001 Private member accessed: `_d_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SLF001 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-05-26 17:57_

---

_Closed by @charliermarsh on 2024-05-26 17:57_

---

_Branch deleted on 2024-05-26 17:57_

---
