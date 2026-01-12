```yaml
number: 8534
title: "[TRIO] Add TRIO109 rule"
type: pull_request
state: merged
author: karpetrosyan
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-trio-109-support
created_at: 2023-11-07T06:59:16Z
updated_at: 2023-11-07T22:13:27Z
url: https://github.com/astral-sh/ruff/pull/8534
synced_at: 2026-01-12T15:55:26Z
```

# [TRIO] Add TRIO109 rule

---

_@karpetrosyan_

## Summary

Adds TRIO109 from the [flake8-trio plugin](https://github.com/Zac-HD/flake8-trio).
Relates to: https://github.com/astral-sh/ruff/issues/8451

---

_Comment by @github-actions[bot] on 2023-11-07 07:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+24 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/bigquery_dts.py#L325'>airflow/providers/google/cloud/hooks/bigquery_dts.py:325:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/cloud_build.py#L649'>airflow/providers/google/cloud/hooks/cloud_build.py:649:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/cloud_composer.py#L336'>airflow/providers/google/cloud/hooks/cloud_composer.py:336:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/cloud_composer.py#L365'>airflow/providers/google/cloud/hooks/cloud_composer.py:365:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/cloud_composer.py#L393'>airflow/providers/google/cloud/hooks/cloud_composer.py:393:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataplex.py#L900'>airflow/providers/google/cloud/hooks/dataplex.py:900:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1136'>airflow/providers/google/cloud/hooks/dataproc.py:1136:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1203'>airflow/providers/google/cloud/hooks/dataproc.py:1203:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1246'>airflow/providers/google/cloud/hooks/dataproc.py:1246:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1281'>airflow/providers/google/cloud/hooks/dataproc.py:1281:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1313'>airflow/providers/google/cloud/hooks/dataproc.py:1313:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1353'>airflow/providers/google/cloud/hooks/dataproc.py:1353:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1433'>airflow/providers/google/cloud/hooks/dataproc.py:1433:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1469'>airflow/providers/google/cloud/hooks/dataproc.py:1469:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1515'>airflow/providers/google/cloud/hooks/dataproc.py:1515:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1558'>airflow/providers/google/cloud/hooks/dataproc.py:1558:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1592'>airflow/providers/google/cloud/hooks/dataproc.py:1592:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1628'>airflow/providers/google/cloud/hooks/dataproc.py:1628:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1662'>airflow/providers/google/cloud/hooks/dataproc.py:1662:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1707'>airflow/providers/google/cloud/hooks/dataproc.py:1707:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1741'>airflow/providers/google/cloud/hooks/dataproc.py:1741:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/dataproc.py#L1777'>airflow/providers/google/cloud/hooks/dataproc.py:1777:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/pubsub.py#L605'>airflow/providers/google/cloud/hooks/pubsub.py:605:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
+ <a href='https://github.com/apache/airflow/blob/637ec90dfca86e57c8c0dbae64af1865db22eb9c/airflow/providers/google/cloud/hooks/pubsub.py#L659'>airflow/providers/google/cloud/hooks/pubsub.py:659:9:</a> TRIO109 Prefer `trio.fail_after` and `trio.move_on_after` over manual `async` timeout behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRIO109 | 24 | 24 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2023-11-07 21:49_

---

_@charliermarsh approved on 2023-11-07 21:50_

---

_Comment by @charliermarsh on 2023-11-07 21:54_

@karpetrosyan - Can you imagine any strategy for narrowing false positives here? For example, could we require that `trio` has been imported?

---

_Comment by @charliermarsh on 2023-11-07 22:05_

I'm gonna add that heuristic for now.

---

_Merged by @charliermarsh on 2023-11-07 22:13_

---

_Closed by @charliermarsh on 2023-11-07 22:13_

---

_Label `rule` added by @charliermarsh on 2023-11-07 22:13_

---

_Label `preview` added by @charliermarsh on 2023-11-07 22:13_

---

_Comment by @charliermarsh on 2023-11-07 22:13_

Gonna add in a follow-up.

---
