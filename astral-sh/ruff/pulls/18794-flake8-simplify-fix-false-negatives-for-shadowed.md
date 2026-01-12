```yaml
number: 18794
title: "[`flake8-simplify`] Fix false negatives for shadowed bindings  (`SIM910`, `SIM911`)"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-SIM911pt2
created_at: 2025-06-19T14:38:24Z
updated_at: 2025-06-20T19:27:14Z
url: https://github.com/astral-sh/ruff/pull/18794
synced_at: 2026-01-12T15:56:25Z
```

# [`flake8-simplify`] Fix false negatives for shadowed bindings  (`SIM910`, `SIM911`)

---

_@LaBatata101_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
I also noticed that the tests for SIM911 were note being run, so I fixed that.

Fixes #18777
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-19 14:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5e74ec4a9e494851be6e695e5e741e7a4257ea14/providers/google/src/airflow/providers/google/cloud/log/stackdriver_task_handler.py#L233'>providers/google/src/airflow/providers/google/cloud/log/stackdriver_task_handler.py:233:27:</a> SIM910 [*] Use `metadata.get("next_page_token")` instead of `metadata.get("next_page_token", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/a0b5565324646ccf1419c2ce8cb465d18514113b/mlflow/pyfunc/loaders/chat_agent.py#L66'>mlflow/pyfunc/loaders/chat_agent.py:66:25:</a> SIM910 [*] Use `dict_input.get("custom_inputs")` instead of `dict_input.get("custom_inputs", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/f866c28f79e19911da923f8d8bf62a0d86cfcea1/astropy/coordinates/tests/test_sky_coord.py#L158'>astropy/coordinates/tests/test_sky_coord.py:158:12:</a> SIM910 [*] Use `attrs0.get(attrnm)` instead of `attrs0.get(attrnm, None)`
+ <a href='https://github.com/astropy/astropy/blob/f866c28f79e19911da923f8d8bf62a0d86cfcea1/astropy/utils/data.py#L1780'>astropy/utils/data.py:1780:25:</a> SIM910 [*] Use `sources.get(u)` instead of `sources.get(u, None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5e74ec4a9e494851be6e695e5e741e7a4257ea14/providers/google/src/airflow/providers/google/cloud/log/stackdriver_task_handler.py#L233'>providers/google/src/airflow/providers/google/cloud/log/stackdriver_task_handler.py:233:27:</a> SIM910 [*] Use `metadata.get("next_page_token")` instead of `metadata.get("next_page_token", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/a0b5565324646ccf1419c2ce8cb465d18514113b/mlflow/pyfunc/loaders/chat_agent.py#L66'>mlflow/pyfunc/loaders/chat_agent.py:66:25:</a> SIM910 [*] Use `dict_input.get("custom_inputs")` instead of `dict_input.get("custom_inputs", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/f866c28f79e19911da923f8d8bf62a0d86cfcea1/astropy/coordinates/tests/test_sky_coord.py#L158'>astropy/coordinates/tests/test_sky_coord.py:158:12:</a> SIM910 [*] Use `attrs0.get(attrnm)` instead of `attrs0.get(attrnm, None)`
+ <a href='https://github.com/astropy/astropy/blob/f866c28f79e19911da923f8d8bf62a0d86cfcea1/astropy/utils/data.py#L1780'>astropy/utils/data.py:1780:25:</a> SIM910 [*] Use `sources.get(u)` instead of `sources.get(u, None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @ntBre on 2025-06-20 17:15_

---

_@ntBre approved on 2025-06-20 17:24_

Nice, this makes sense to me. And good catch on the tests not running!

I'll update the title to be something about shadowing. As the ecosystem report shows, this wasn't actually specific to the `dict` name, just that `dict` was shadowing the builtin and thus not the `only_binding`.

---

_Renamed from "[`flake8-simplify`] Fix false positives in `SIM910` and `SIM911` if variable is named `dict`" to "[`flake8-simplify`] Fix false negatives for shadowed bindings  (`SIM910` , `SIM911`)" by @ntBre on 2025-06-20 17:24_

---

_Renamed from "[`flake8-simplify`] Fix false negatives for shadowed bindings  (`SIM910` , `SIM911`)" to "[`flake8-simplify`] Fix false negatives for shadowed bindings  (`SIM910`, `SIM911`)" by @ntBre on 2025-06-20 17:25_

---

_Merged by @ntBre on 2025-06-20 17:25_

---

_Closed by @ntBre on 2025-06-20 17:25_

---

_Branch deleted on 2025-06-20 19:27_

---
