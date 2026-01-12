```yaml
number: 14474
title: "[`pycodestyle`] Exempt `pytest.importorskip()` calls (`E402`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: E402
created_at: 2024-11-20T02:26:19Z
updated_at: 2024-11-20T03:17:32Z
url: https://github.com/astral-sh/ruff/pull/14474
synced_at: 2026-01-12T15:55:47Z
```

# [`pycodestyle`] Exempt `pytest.importorskip()` calls (`E402`)

---

_@InSyncWithFoo_

## Summary

Resolves #13537.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-20 02:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/61315111b6f581823b36ce123c444c8403bc6032/providers/tests/weaviate/operators/test_weaviate.py#L28'>providers/tests/weaviate/operators/test_weaviate.py:28:62:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/impala/tests/test_ddl.py#L18'>ibis/backends/impala/tests/test_ddl.py:18:44:</a> RUF100 Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/impala/tests/test_parquet_ddl.py#L12'>ibis/backends/impala/tests/test_parquet_ddl.py:12:44:</a> RUF100 Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/impala/tests/test_partition.py#L15'>ibis/backends/impala/tests/test_partition.py:15:48:</a> RUF100 Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/tests/expr/test_pretty_repr.py#L13'>ibis/tests/expr/test_pretty_repr.py:13:66:</a> RUF100 Unused `noqa` directive (unused: `E402`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-11-20 03:08_

---

_Closed by @charliermarsh on 2024-11-20 03:08_

---

_Label `rule` added by @charliermarsh on 2024-11-20 03:08_

---

_Label `preview` added by @charliermarsh on 2024-11-20 03:08_

---

_Branch deleted on 2024-11-20 03:17_

---
