```yaml
number: 10313
title: "[`flake8-bandit`] Implement upstream updates for `S311`, `S324` and `S605`"
type: pull_request
state: merged
author: mkniewallner
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/update-bandit-rules
created_at: 2024-03-09T12:05:53Z
updated_at: 2024-03-11T21:19:16Z
url: https://github.com/astral-sh/ruff/pull/10313
synced_at: 2026-01-10T22:47:01Z
```

# [`flake8-bandit`] Implement upstream updates for `S311`, `S324` and `S605`

---

_Pull request opened by @mkniewallner on 2024-03-09 12:05_

## Summary

Pick up updates made in latest [releases](https://github.com/PyCQA/bandit/releases) of `bandit`:
- `S311`: https://github.com/PyCQA/bandit/pull/940 and https://github.com/PyCQA/bandit/pull/1096
- `S324`: https://github.com/PyCQA/bandit/pull/1018
- `S605`: https://github.com/PyCQA/bandit/pull/1116

## Test Plan

Snapshot tests

---

_Comment by @github-actions[bot] on 2024-03-09 12:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/backoff.py#L45'>disnake/backoff.py:45:16:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/colour.py#L135'>disnake/colour.py:135:44:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ac5f72022e24428408dddc08d68a5836674afc08/airflow/dag_processing/manager.py#L1156'>airflow/dag_processing/manager.py:1156:13:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/apache/airflow/blob/ac5f72022e24428408dddc08d68a5836674afc08/tests/dag_processing/test_job_runner.py#L363'>tests/dag_processing/test_job_runner.py:363:9:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/model-bakers/model_bakery/blob/c972cbb24f8c1fabfd285112588a72bd282d9014/model_bakery/random_gen.py#L30'>model_bakery/random_gen.py:30:16:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/03ec788a9147db72729f867452342354ab5bea60/analytics/lib/fixtures.py#L39'>analytics/lib/fixtures.py:39:11:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/zulip/zulip/blob/03ec788a9147db72729f867452342354ab5bea60/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/zulip/zulip/blob/03ec788a9147db72729f867452342354ab5bea60/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S607 Starting a process with a partial executable path
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S311 | 6 | 6 | 0 | 0 | 0 |
| S605 | 1 | 1 | 0 | 0 | 0 |
| S607 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/backoff.py#L45'>disnake/backoff.py:45:16:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/colour.py#L135'>disnake/colour.py:135:44:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ac5f72022e24428408dddc08d68a5836674afc08/airflow/dag_processing/manager.py#L1156'>airflow/dag_processing/manager.py:1156:13:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/apache/airflow/blob/ac5f72022e24428408dddc08d68a5836674afc08/tests/dag_processing/test_job_runner.py#L363'>tests/dag_processing/test_job_runner.py:363:9:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/model-bakers/model_bakery/blob/c972cbb24f8c1fabfd285112588a72bd282d9014/model_bakery/random_gen.py#L30'>model_bakery/random_gen.py:30:16:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/03ec788a9147db72729f867452342354ab5bea60/analytics/lib/fixtures.py#L39'>analytics/lib/fixtures.py:39:11:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/zulip/zulip/blob/03ec788a9147db72729f867452342354ab5bea60/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/zulip/zulip/blob/03ec788a9147db72729f867452342354ab5bea60/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S607 Starting a process with a partial executable path
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S311 | 6 | 6 | 0 | 0 | 0 |
| S605 | 1 | 1 | 0 | 0 | 0 |
| S607 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @mkniewallner on 2024-03-09 13:59_

---

_Comment by @mkniewallner on 2024-03-09 14:01_

Not sure why there is a new match for `S607` on `zulip` in the ecosystem check as it was not changed.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-11 20:48_

---

_Label `rule` added by @charliermarsh on 2024-03-11 20:48_

---

_@charliermarsh approved on 2024-03-11 20:55_

Nice, this looks great -- thanks!

---

_Comment by @charliermarsh on 2024-03-11 20:56_

> Not sure why there is a new match for S607 on zulip in the ecosystem check as it was not changed.

I think it's because you added `getstatusoutput` to the list of shell-like calls.

---

_Merged by @charliermarsh on 2024-03-11 21:07_

---

_Closed by @charliermarsh on 2024-03-11 21:07_

---

_Comment by @mkniewallner on 2024-03-11 21:11_

> > Not sure why there is a new match for S607 on zulip in the ecosystem check as it was not changed.
> 
> I think it's because you added `getstatusoutput` to the list of shell-like calls.

Oh yeah sorry, I did not know what S607 was exactly, and didn't see that this change would also apply to this rule. But it does seem legit to handle this new case for S607, as this is the core logic to detect shell invocations, so all good it seems!

---

_Branch deleted on 2024-03-11 21:11_

---
