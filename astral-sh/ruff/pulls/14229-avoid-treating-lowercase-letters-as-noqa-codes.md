```yaml
number: 14229
title: "Avoid treating lowercase letters as `# noqa` codes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/b
created_at: 2024-11-09T17:38:40Z
updated_at: 2024-11-15T10:53:46Z
url: https://github.com/astral-sh/ruff/pull/14229
synced_at: 2026-01-12T15:55:47Z
```

# Avoid treating lowercase letters as `# noqa` codes

---

_@charliermarsh_

## Summary

An oversight from the original implementation.

Closes https://github.com/astral-sh/ruff/issues/14228.


---

_Renamed from "Avoid treating lowercase letters as noqa codes" to "Avoid treating lowercase letters as `# noqa` codes" by @charliermarsh on 2024-11-09 17:38_

---

_@charliermarsh reviewed on 2024-11-09 17:38_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/snapshots/ruff_linter__noqa__tests__noqa_squashed_codes.snap`:17 on 2024-11-09 17:38_

This seems "fine", it's not ambiguous.

---

_Merged by @charliermarsh on 2024-11-09 17:49_

---

_Closed by @charliermarsh on 2024-11-09 17:49_

---

_Branch deleted on 2024-11-09 17:49_

---

_Comment by @github-actions[bot] on 2024-11-09 17:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/de8818270095d9f05edfec8e03daa5df7d6a773b/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L45'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:45:37:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
- <a href='https://github.com/apache/airflow/blob/de8818270095d9f05edfec8e03daa5df7d6a773b/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L50'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:50:38:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
- <a href='https://github.com/apache/airflow/blob/de8818270095d9f05edfec8e03daa5df7d6a773b/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L59'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:59:42:</a> RUF100 [*] Unused `noqa` directive (unknown: `Used`)
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
| RUF100 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/de8818270095d9f05edfec8e03daa5df7d6a773b/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L45'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:45:37:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
- <a href='https://github.com/apache/airflow/blob/de8818270095d9f05edfec8e03daa5df7d6a773b/providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py#L50'>providers/src/airflow/providers/apache/hdfs/sensors/hdfs.py:50:38:</a> RUF100 [*] Unused `noqa` directive (unknown: `Ignore`)
- <a href='https://github.com/apache/airflow/blob/de8818270095d9f05edfec8e03daa5df7d6a773b/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L59'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py:59:42:</a> RUF100 [*] Unused `noqa` directive (unknown: `Used`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---

_@InSyncWithFoo reviewed on 2024-11-09 18:05_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/noqa.rs`:186 on 2024-11-09 18:05_

What about `ParsedFileExemption::lex_code()`?

There are multiple reimplementations of the `# noqa` parsing algorithm, including two by me ([RyeCharm](https://github.com/InSyncWithFoo/ryecharm/blob/b239c79cadc11e2f7a752d7071bba521f35a96f1/src/main/kotlin/insyncwithfoo/ryecharm/ruff/documentation/noqa/NoqaComment.kt#L15-L39), #14111) and another by @koxudaxi ([Ruff PyCharm Plugin](https://github.com/koxudaxi/ruff-pycharm-plugin/blob/f0feae5b93ddf53e0fc1504d24b82780614ac73e/src/com/koxudaxi/ruff/RuffNoqaDocumentationTarget.kt#L57-L65)). I would appreciate it if you could keep it consistent.

---

_Label `bug` added by @dhruvmanila on 2024-11-15 10:53_

---
