```yaml
number: 5749
title: "Ignore `Enum`-and-`str` subclasses for slots enforcement"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/enum
created_at: 2023-07-13T20:04:47Z
updated_at: 2023-07-13T20:35:30Z
url: https://github.com/astral-sh/ruff/pull/5749
synced_at: 2026-01-12T03:30:21Z
```

# Ignore `Enum`-and-`str` subclasses for slots enforcement

---

_Pull request opened by @charliermarsh on 2023-07-13 20:04_

## Summary

Matches the behavior of the upstream plugin.

Closes #5748.

---

_Label `bug` added by @charliermarsh on 2023-07-13 20:05_

---

_@zanieb approved on 2023-07-13 20:10_

---

_Merged by @charliermarsh on 2023-07-13 20:12_

---

_Closed by @charliermarsh on 2023-07-13 20:12_

---

_Branch deleted on 2023-07-13 20:12_

---

_Comment by @github-actions[bot] on 2023-07-13 20:15_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -14, 0 error(s))

<details><summary>airflow (+0, -14)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/models/dagwarning.py#L95'>airflow/models/dagwarning.py:95:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/providers/amazon/aws/hooks/dms.py#L26'>airflow/providers/amazon/aws/hooks/dms.py:26:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/providers/cncf/kubernetes/triggers/pod.py#L35'>airflow/providers/cncf/kubernetes/triggers/pod.py:35:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/providers/google/cloud/hooks/dataprep.py#L47'>airflow/providers/google/cloud/hooks/dataprep.py:47:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/providers/google/cloud/utils/dataform.py#L31'>airflow/providers/google/cloud/utils/dataform.py:31:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/serialization/enums.py#L26'>airflow/serialization/enums.py:26:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/serialization/enums.py#L35'>airflow/serialization/enums.py:35:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/log/file_task_handler.py#L49'>airflow/utils/log/file_task_handler.py:49:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/state.py#L23'>airflow/utils/state.py:23:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/state.py#L36'>airflow/utils/state.py:36:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/state.py#L67'>airflow/utils/state.py:67:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/trigger_rule.py#L23'>airflow/utils/trigger_rule.py:23:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/types.py#L47'>airflow/utils/types.py:47:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
- <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/utils/weight_rule.py#L25'>airflow/utils/weight_rule.py:25:7:</a> SLOT000 Subclasses of `str` should define `__slots__`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SLOT000 | 14 | 0 | 14 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.03ms     4.3 MB/sec    1.02      9.7±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.5 MB/sec    1.01      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.06   262.9±12.67µs    11.2 MB/sec    1.00    248.5±1.02µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.23ms     5.2 MB/sec    1.03      5.1±0.20ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.08ms     2.5 MB/sec    1.00     16.2±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    535.4±2.27µs     5.5 MB/sec    1.00    537.0±1.27µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.10ms     3.5 MB/sec    1.00      7.3±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1756.9±7.26µs     9.5 MB/sec    1.00   1750.2±5.03µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   208.6±12.09µs    14.1 MB/sec    1.01    209.7±8.87µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     13.4±0.55ms     3.0 MB/sec    1.00     13.3±0.55ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.01      3.0±0.12ms     5.6 MB/sec    1.00      2.9±0.14ms     5.7 MB/sec
formatter/numpy/globals.py                 1.05  354.5±147.77µs     8.3 MB/sec    1.00   336.3±17.94µs     8.8 MB/sec
formatter/pydantic/types.py                1.01      6.5±0.27ms     3.9 MB/sec    1.00      6.5±0.23ms     4.0 MB/sec
linter/all-rules/large/dataset.py          1.03     23.7±1.21ms  1760.8 KB/sec    1.00     23.0±0.70ms  1808.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      6.0±0.35ms     2.8 MB/sec    1.00      5.9±0.24ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   698.7±36.14µs     4.2 MB/sec    1.00   701.7±46.74µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.05     10.6±0.60ms     2.4 MB/sec    1.00     10.2±0.35ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.02     11.8±0.38ms     3.5 MB/sec    1.00     11.5±0.43ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.4±0.08ms     6.9 MB/sec    1.00      2.4±0.09ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   297.1±14.38µs     9.9 MB/sec    1.00   296.3±19.07µs    10.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      5.3±0.28ms     4.8 MB/sec    1.00      5.1±0.16ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
