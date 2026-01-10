```yaml
number: 10909
title: "Avoid `non-augmented-assignment` for reversed, non-commutative operators"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comm
created_at: 2024-04-12T13:51:28Z
updated_at: 2024-04-12T14:04:58Z
url: https://github.com/astral-sh/ruff/pull/10909
synced_at: 2026-01-10T22:37:01Z
```

# Avoid `non-augmented-assignment` for reversed, non-commutative operators

---

_Pull request opened by @charliermarsh on 2024-04-12 13:51_

Closes https://github.com/astral-sh/ruff/issues/10900.

---

_Label `bug` added by @charliermarsh on 2024-04-12 13:51_

---

_@dhruvmanila approved on 2024-04-12 14:00_

---

_Comment by @github-actions[bot] on 2024-04-12 14:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -11 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c3bb80da939025dd49b646a819f5e984faf9ddfc/airflow/models/taskinstance.py#L1636'>airflow/models/taskinstance.py:1636:21:</a> PLR6104 Use `/=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/c3bb80da939025dd49b646a819f5e984faf9ddfc/airflow/providers/fab/auth_manager/security_manager/override.py#L2041'>airflow/providers/fab/auth_manager/security_manager/override.py:2041:21:</a> PLR6104 Use `%=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_util.py#L462'>securedrop/pretty_bad_protocol/_util.py:462:5:</a> PLR6104 Use `%=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/groupby/groupby.py#L1973'>pandas/core/groupby/groupby.py:1973:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/groupby/groupby.py#L4502'>pandas/core/groupby/groupby.py:4502:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/indexes/range.py#L455'>pandas/core/indexes/range.py:455:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/methods/selectn.py#L171'>pandas/core/methods/selectn.py:171:13:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/io/pytables.py#L4206'>pandas/io/pytables.py:4206:29:</a> PLR6104 Use `-=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_timedelta64.py#L1615'>pandas/tests/arithmetic/test_timedelta64.py:1615:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_timedelta64.py#L1655'>pandas/tests/arithmetic/test_timedelta64.py:1655:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_timedelta64.py#L1683'>pandas/tests/arithmetic/test_timedelta64.py:1683:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6104 | 11 | 0 | 11 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-12 14:04_

---

_Closed by @charliermarsh on 2024-04-12 14:04_

---

_Branch deleted on 2024-04-12 14:04_

---
