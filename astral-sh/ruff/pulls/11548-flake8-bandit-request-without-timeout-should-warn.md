```yaml
number: 11548
title: "[`flake8-bandit`] `request-without-timeout` should warn for `requests.request`"
type: pull_request
state: merged
author: akshetpandey
labels:
  - rule
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-05-26T19:58:15Z
updated_at: 2024-05-28T16:46:25Z
url: https://github.com/astral-sh/ruff/pull/11548
synced_at: 2026-01-12T15:55:38Z
```

# [`flake8-bandit`] `request-without-timeout` should warn for `requests.request`

---

_@akshetpandey_

## Summary
Update [S113](https://docs.astral.sh/ruff/rules/request-without-timeout/) to also warns for missing timeout on when calling `requests.request`

---

_Label `rule` added by @charliermarsh on 2024-05-26 23:28_

---

_Comment by @charliermarsh on 2024-05-27 16:55_

This seems reasonable though it's surprising that Bandit itself does not check this method.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 13:57_

---

_Comment by @github-actions[bot] on 2024-05-28 16:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/dad3c590346b2d9e2cc86723b4c5ff3b5a43c000/docker_tests/test_docker_compose_quick_start.py#L44'>docker_tests/test_docker_compose_quick_start.py:44:16:</a> S113 Probable use of requests call without timeout
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/aa84080ad63a28185aeeb6b4165da8413521da8b/zerver/tests/test_internet.py#L14'>zerver/tests/test_internet.py:14:22:</a> S113 Probable use of requests call without timeout
+ <a href='https://github.com/zulip/zulip/blob/aa84080ad63a28185aeeb6b4165da8413521da8b/zerver/tests/test_internet.py#L26'>zerver/tests/test_internet.py:26:22:</a> S113 Probable use of requests call without timeout
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S113 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/dad3c590346b2d9e2cc86723b4c5ff3b5a43c000/docker_tests/test_docker_compose_quick_start.py#L44'>docker_tests/test_docker_compose_quick_start.py:44:16:</a> S113 Probable use of requests call without timeout
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/aa84080ad63a28185aeeb6b4165da8413521da8b/zerver/tests/test_internet.py#L14'>zerver/tests/test_internet.py:14:22:</a> S113 Probable use of requests call without timeout
+ <a href='https://github.com/zulip/zulip/blob/aa84080ad63a28185aeeb6b4165da8413521da8b/zerver/tests/test_internet.py#L26'>zerver/tests/test_internet.py:26:22:</a> S113 Probable use of requests call without timeout
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S113 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "S113 request-without-timeout should warn for `requests.request` without timeout" to "[`flake8-bandit`] `request-without-timeout` should warn for `requests.request`" by @charliermarsh on 2024-05-28 16:27_

---

_@charliermarsh approved on 2024-05-28 16:28_

---

_Merged by @charliermarsh on 2024-05-28 16:31_

---

_Closed by @charliermarsh on 2024-05-28 16:31_

---
