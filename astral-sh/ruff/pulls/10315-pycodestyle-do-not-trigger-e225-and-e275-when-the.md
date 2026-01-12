```yaml
number: 10315
title: "[`pycodestyle`] Do not trigger `E225` and `E275` when the next token is a ')'"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - bug
assignees: []
merged: true
base: main
head: hoel/fix_10295
created_at: 2024-03-09T15:42:10Z
updated_at: 2024-03-11T22:26:12Z
url: https://github.com/astral-sh/ruff/pull/10315
synced_at: 2026-01-12T15:55:31Z
```

# [`pycodestyle`] Do not trigger `E225` and `E275` when the next token is a ')'

---

_@hoel-bagard_

## Summary

Fixes #10295.

`E225` (`Missing whitespace around operator`) and `E275` (`Missing whitespace after keyword`) try to add a white space even when the next character is a `)` (which is a syntax error in most cases, the exceptions already being handled). This causes `E202` (`Whitespace before close bracket`) to try to remove the added whitespace, resulting in an infinite loop when `E225`/`E275` re-add it.
This PR adds an exception in `E225` and `E275` to not trigger in case the next token is a `)`. It is a bit simplistic, but it solves the example given in the issue without introducing a change in behavior (according to the fixtures).

## Test Plan

`cargo test` and the `ruff-ecosystem` check were used to check that the PR's changes do not have side-effects.
A new fixture was added to check that running the 3 rules on the example given in the issue does not cause ruff to fail to converge.

---

_Comment by @github-actions[bot] on 2024-03-09 16:29_

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
+ <a href='https://github.com/apache/airflow/blob/ccc9bb5f6e3f1a1915cf71fcc178bc94e6264de6/airflow/dag_processing/manager.py#L1156'>airflow/dag_processing/manager.py:1156:13:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/apache/airflow/blob/ccc9bb5f6e3f1a1915cf71fcc178bc94e6264de6/tests/dag_processing/test_job_runner.py#L363'>tests/dag_processing/test_job_runner.py:363:9:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
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
+ <a href='https://github.com/zulip/zulip/blob/73370087ed94349af1440d83fe67a796679de016/analytics/lib/fixtures.py#L39'>analytics/lib/fixtures.py:39:11:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/zulip/zulip/blob/73370087ed94349af1440d83fe67a796679de016/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/zulip/zulip/blob/73370087ed94349af1440d83fe67a796679de016/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S607 Starting a process with a partial executable path
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
+ <a href='https://github.com/apache/airflow/blob/ccc9bb5f6e3f1a1915cf71fcc178bc94e6264de6/airflow/dag_processing/manager.py#L1156'>airflow/dag_processing/manager.py:1156:13:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/apache/airflow/blob/ccc9bb5f6e3f1a1915cf71fcc178bc94e6264de6/tests/dag_processing/test_job_runner.py#L363'>tests/dag_processing/test_job_runner.py:363:9:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
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
+ <a href='https://github.com/zulip/zulip/blob/73370087ed94349af1440d83fe67a796679de016/analytics/lib/fixtures.py#L39'>analytics/lib/fixtures.py:39:11:</a> S311 Standard pseudo-random generators are not suitable for cryptographic purposes
+ <a href='https://github.com/zulip/zulip/blob/73370087ed94349af1440d83fe67a796679de016/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/zulip/zulip/blob/73370087ed94349af1440d83fe67a796679de016/tools/lib/provision.py#L280'>tools/lib/provision.py:280:60:</a> S607 Starting a process with a partial executable path
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

_Marked ready for review by @hoel-bagard on 2024-03-10 02:19_

---

_@charliermarsh approved on 2024-03-11 21:16_

---

_@charliermarsh reviewed on 2024-03-11 21:16_

That's great, thanks!

---

_Label `bug` added by @charliermarsh on 2024-03-11 21:16_

---

_Merged by @charliermarsh on 2024-03-11 21:23_

---

_Closed by @charliermarsh on 2024-03-11 21:23_

---

_Branch deleted on 2024-03-11 22:26_

---
