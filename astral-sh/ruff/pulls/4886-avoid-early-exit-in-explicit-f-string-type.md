```yaml
number: 4886
title: Avoid early-exit in explicit-f-string-type-conversion
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ruf010
created_at: 2023-06-06T00:45:57Z
updated_at: 2023-06-06T01:17:18Z
url: https://github.com/astral-sh/ruff/pull/4886
synced_at: 2026-01-12T15:55:16Z
```

# Avoid early-exit in explicit-f-string-type-conversion

---

_@charliermarsh_

## Summary

All these `return` statements should've been `continue` statements. Using `return` meant that if a string contained multiple expression, like `f"{bla!s}, {(repr(bla))}, {(ascii(bla))}"`, and one of them was already modified to an explicit conversion (like the first), we'd skip fixing the remaining.

Closes #4567.


---

_Label `bug` added by @charliermarsh on 2023-06-06 00:46_

---

_Merged by @charliermarsh on 2023-06-06 00:52_

---

_Closed by @charliermarsh on 2023-06-06 00:52_

---

_Branch deleted on 2023-06-06 00:52_

---

_Comment by @github-actions[bot] on 2023-06-06 00:55_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+20, -0, 0 error(s))

<details><summary>airflow (+12, -0)</summary>
<p>

```diff
+ airflow/exceptions.py:299:49: RUF010 [*] Use conversion in f-string
+ airflow/policies.py:170:43: RUF010 [*] Use conversion in f-string
+ airflow/providers/amazon/aws/log/cloudwatch_task_handler.py:99:40: RUF010 Use conversion in f-string
+ airflow/providers/apache/livy/triggers/livy.py:104:80: RUF010 [*] Use conversion in f-string
+ airflow/providers/apache/spark/hooks/spark_submit.py:270:51: RUF010 [*] Use conversion in f-string
+ airflow/providers/cncf/kubernetes/utils/delete_from.py:140:42: RUF010 [*] Use conversion in f-string
+ airflow/providers/sftp/operators/sftp.py:188:83: RUF010 [*] Use conversion in f-string
+ airflow/task/task_runner/cgroup_task_runner.py:137:84: RUF010 [*] Use conversion in f-string
+ tests/integration/executors/test_celery_executor.py:246:56: RUF010 [*] Use conversion in f-string
+ tests/models/test_baseoperator.py:53:56: RUF010 [*] Use conversion in f-string
+ tests/models/test_renderedtifields.py:51:56: RUF010 [*] Use conversion in f-string
+ tests/serialization/test_dag_serialization.py:1055:49: RUF010 [*] Use conversion in f-string
```

</p>
</details>
<details><summary>zulip (+8, -0)</summary>
<p>

```diff
+ zerver/lib/data_types.py:107:33: RUF010 [*] Use conversion in f-string
+ zerver/lib/queue.py:97:32: RUF010 [*] Use conversion in f-string
+ zerver/lib/test_classes.py:1068:62: RUF010 [*] Use conversion in f-string
+ zerver/lib/test_classes.py:1192:45: RUF010 [*] Use conversion in f-string
+ zerver/tests/test_openapi.py:385:31: RUF010 [*] Use conversion in f-string
+ zerver/tests/test_openapi.py:385:50: RUF010 [*] Use conversion in f-string
+ zerver/tests/test_push_notifications.py:402:104: RUF010 Use conversion in f-string
+ zerver/tests/test_push_notifications.py:404:129: RUF010 Use conversion in f-string
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF010 | 20 | 20 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      5.8±0.03ms     7.1 MB/sec    1.00      5.7±0.01ms     7.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1194.9±1.35µs    13.9 MB/sec    1.00   1191.1±3.09µs    14.0 MB/sec
formatter/numpy/globals.py                 1.00    137.9±0.29µs    21.4 MB/sec    1.00    137.6±0.12µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms    10.0 MB/sec    1.00      2.5±0.01ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.01     14.0±0.09ms     2.9 MB/sec    1.00     13.9±0.14ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.01      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.6±0.47µs     7.2 MB/sec    1.01    415.0±0.40µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.00      5.8±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1437.0±2.46µs    11.6 MB/sec    1.02   1462.7±3.25µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.1±0.24µs    18.3 MB/sec    1.02    164.7±0.18µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.03      7.0±0.33ms     5.8 MB/sec     1.00      6.8±0.05ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.15  1594.0±152.47µs    10.4 MB/sec    1.00  1383.1±17.90µs    12.0 MB/sec
formatter/numpy/globals.py                 1.10   173.6±16.45µs    17.0 MB/sec     1.00    158.0±3.17µs    18.7 MB/sec
formatter/pydantic/types.py                1.14      3.5±0.28ms     7.4 MB/sec     1.00      3.0±0.05ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.12     18.6±0.47ms     2.2 MB/sec     1.00     16.6±0.09ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.14      4.7±0.23ms     3.5 MB/sec     1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.09   525.7±29.47µs     5.6 MB/sec     1.00    483.3±5.62µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.11      7.8±0.22ms     3.3 MB/sec     1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.05      9.0±0.31ms     4.5 MB/sec     1.00      8.6±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1854.2±47.04µs     9.0 MB/sec     1.00  1775.7±17.02µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.9±6.82µs    14.9 MB/sec     1.00    198.4±6.25µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.15ms     6.4 MB/sec     1.00      3.8±0.06ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
