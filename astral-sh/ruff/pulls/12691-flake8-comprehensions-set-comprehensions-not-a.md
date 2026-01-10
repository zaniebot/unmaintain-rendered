```yaml
number: 12691
title: "[flake8-comprehensions] Set comprehensions not a violation for `sum` in `unnecessary-comprehension-in-call` (`C419`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: c419-set-comprehension
created_at: 2024-08-05T16:34:35Z
updated_at: 2024-08-06T02:49:07Z
url: https://github.com/astral-sh/ruff/pull/12691
synced_at: 2026-01-10T21:47:02Z
```

# [flake8-comprehensions] Set comprehensions not a violation for `sum` in `unnecessary-comprehension-in-call` (`C419`)

---

_Pull request opened by @dylwil3 on 2024-08-05 16:34_

## Summary

Removes set comprehension as a violation for `sum` when checking `C419`, because set comprehension may de-duplicate entries in a generator, thereby modifying the value of the sum.

Closes #12690.


---

_Comment by @github-actions[bot] on 2024-08-05 16:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fdda4478e5829c08aa5059f8ad86f92922313d19/tests/api_connexion/endpoints/test_event_log_endpoint.py#L367'>tests/api_connexion/endpoints/test_event_log_endpoint.py:367:24:</a> C419 Unnecessary list comprehension
+ <a href='https://github.com/apache/airflow/blob/fdda4478e5829c08aa5059f8ad86f92922313d19/tests/api_connexion/endpoints/test_event_log_endpoint.py#L367'>tests/api_connexion/endpoints/test_event_log_endpoint.py:367:24:</a> C419 Unnecessary set comprehension
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C419 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fdda4478e5829c08aa5059f8ad86f92922313d19/tests/api_connexion/endpoints/test_event_log_endpoint.py#L367'>tests/api_connexion/endpoints/test_event_log_endpoint.py:367:24:</a> C419 Unnecessary list comprehension
+ <a href='https://github.com/apache/airflow/blob/fdda4478e5829c08aa5059f8ad86f92922313d19/tests/api_connexion/endpoints/test_event_log_endpoint.py#L367'>tests/api_connexion/endpoints/test_event_log_endpoint.py:367:24:</a> C419 Unnecessary set comprehension
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C419 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-08-06 02:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension_in_call.rs`:80 on 2024-08-06 02:25_

I changed this to a manual match rather than using `Display` for the (bad) reason that we use the first `format!` call here to show the example error message in the docs, and "Unnecessary list comprehension" is better than "Unnecessary {} comprehension".

---

_@charliermarsh approved on 2024-08-06 02:25_

---

_Label `bug` added by @charliermarsh on 2024-08-06 02:25_

---

_Merged by @charliermarsh on 2024-08-06 02:30_

---

_Closed by @charliermarsh on 2024-08-06 02:30_

---

_Branch deleted on 2024-08-06 02:49_

---
