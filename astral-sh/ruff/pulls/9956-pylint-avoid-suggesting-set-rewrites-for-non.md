```yaml
number: 9956
title: "[`pylint`] Avoid suggesting set rewrites for non-hashable types"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/plr
created_at: 2024-02-12T17:17:46Z
updated_at: 2024-02-12T18:05:55Z
url: https://github.com/astral-sh/ruff/pull/9956
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Avoid suggesting set rewrites for non-hashable types

---

_Pull request opened by @charliermarsh on 2024-02-12 17:17_

## Summary

Ensures that `x in [y, z]` does not trigger in `x`, `y`, or `z` are known _not_ to be hashable.

Closes https://github.com/astral-sh/ruff/issues/9928.



---

_Label `bug` added by @charliermarsh on 2024-02-12 17:17_

---

_Marked ready for review by @charliermarsh on 2024-02-12 17:17_

---

_Comment by @github-actions[bot] on 2024-02-12 17:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -12 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/airflow/providers/google/cloud/hooks/bigquery.py#L2204'>airflow/providers/google/cloud/hooks/bigquery.py:2204:70:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/airflow/providers/google/cloud/hooks/bigquery.py#L2209'>airflow/providers/google/cloud/hooks/bigquery.py:2209:25:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/airflow/providers/google/cloud/hooks/bigquery.py#L2993'>airflow/providers/google/cloud/hooks/bigquery.py:2993:70:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/airflow/providers/google/cloud/hooks/bigquery.py#L2998'>airflow/providers/google/cloud/hooks/bigquery.py:2998:25:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/airflow/serialization/serialized_objects.py#L642'>airflow/serialization/serialized_objects.py:642:69:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/tests/providers/google/cloud/operators/test_stackdriver.py#L191'>tests/providers/google/cloud/operators/test_stackdriver.py:191:26:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/002d15a9f15c7ba3ba5cf5c431afc32bff7ab4e7/tests/utils/test_operator_resources.py#L26'>tests/utils/test_operator_resources.py:26:25:</a> PLR6201 Use a `set` literal when testing for membership
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/pymilvus/client/grpc_handler.py#L1387'>pymilvus/client/grpc_handler.py:1387:32:</a> PLR6201 Use a `set` literal when testing for membership
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/632a41f07b7e60ae6df1ea554303414c7606c0ad/pandas/plotting/_matplotlib/core.py#L183'>pandas/plotting/_matplotlib/core.py:183:18:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/632a41f07b7e60ae6df1ea554303414c7606c0ad/pandas/tseries/frequencies.py#L231'>pandas/tseries/frequencies.py:231:32:</a> PLR6201 Use a `set` literal when testing for membership
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e4c6476c577a106b2a4679a5f89c10bdc82f9d0b/zerver/migrations/0460_backfill_realmauditlog_extradata_to_json_field.py#L119'>zerver/migrations/0460_backfill_realmauditlog_extradata_to_json_field.py:119:29:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/zulip/zulip/blob/e4c6476c577a106b2a4679a5f89c10bdc82f9d0b/zilencer/migrations/0027_backfill_remote_realmauditlog_extradata_to_json_field.py#L74'>zilencer/migrations/0027_backfill_remote_realmauditlog_extradata_to_json_field.py:74:29:</a> PLR6201 Use a `set` literal when testing for membership
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6201 | 12 | 0 | 12 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-02-12 17:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/typing.rs`:430 on 2024-02-12 17:50_

Gonna follow-up on these separately.

---

_Merged by @charliermarsh on 2024-02-12 18:05_

---

_Closed by @charliermarsh on 2024-02-12 18:05_

---

_Branch deleted on 2024-02-12 18:05_

---
