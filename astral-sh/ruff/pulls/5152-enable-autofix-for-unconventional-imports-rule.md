```yaml
number: 5152
title: Enable autofix for unconventional imports rule
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: charlie/fix-import-convention
created_at: 2023-06-16T18:05:19Z
updated_at: 2023-06-18T16:19:11Z
url: https://github.com/astral-sh/ruff/pull/5152
synced_at: 2026-01-12T15:55:17Z
```

# Enable autofix for unconventional imports rule

---

_@charliermarsh_

## Summary

We can now automatically rewrite `import pandas` to `import pandas as pd`, with minimal changes needed.


---

_Review requested from @konstin by @charliermarsh on 2023-06-16 18:05_

---

_Comment by @github-actions[bot] on 2023-06-16 18:18_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+8, -8, 0 error(s))

<details><summary>airflow (+8, -8)</summary>
<p>

```diff
+ airflow/executors/celery_executor_utils.py:88:16: ICN001 [*] `numpy` should be imported as `np`
- airflow/executors/celery_executor_utils.py:88:16: ICN001 `numpy` should be imported as `np`
+ airflow/providers/apache/hive/hooks/hive.py:34:12: ICN001 [*] `pandas` should be imported as `pd`
- airflow/providers/apache/hive/hooks/hive.py:34:12: ICN001 `pandas` should be imported as `pd`
+ airflow/providers/oracle/hooks/oracle.py:29:12: ICN001 [*] `numpy` should be imported as `np`
- airflow/providers/oracle/hooks/oracle.py:29:12: ICN001 `numpy` should be imported as `np`
+ airflow/providers/presto/hooks/presto.py:157:16: ICN001 [*] `pandas` should be imported as `pd`
- airflow/providers/presto/hooks/presto.py:157:16: ICN001 `pandas` should be imported as `pd`
+ airflow/providers/trino/hooks/trino.py:172:16: ICN001 [*] `pandas` should be imported as `pd`
- airflow/providers/trino/hooks/trino.py:172:16: ICN001 `pandas` should be imported as `pd`
+ tests/providers/oracle/hooks/test_oracle.py:24:8: ICN001 [*] `numpy` should be imported as `np`
- tests/providers/oracle/hooks/test_oracle.py:24:8: ICN001 `numpy` should be imported as `np`
+ tests/serialization/serializers/test_serializers.py:22:8: ICN001 [*] `numpy` should be imported as `np`
- tests/serialization/serializers/test_serializers.py:22:8: ICN001 `numpy` should be imported as `np`
+ tests/serialization/serializers/test_serializers.py:23:8: ICN001 [*] `pandas` should be imported as `pd`
- tests/serialization/serializers/test_serializers.py:23:8: ICN001 `pandas` should be imported as `pd`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| ICN001 | 16 | 8 | 8 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.8±0.23ms     5.2 MB/sec    1.00      7.7±0.20ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1631.4±35.46µs    10.2 MB/sec    1.00  1621.2±54.45µs    10.3 MB/sec
formatter/numpy/globals.py                 1.05    171.5±9.27µs    17.2 MB/sec    1.00   163.6±11.93µs    18.0 MB/sec
formatter/pydantic/types.py                1.04      3.3±0.11ms     7.7 MB/sec    1.00      3.2±0.10ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.05     17.8±0.62ms     2.3 MB/sec    1.00     17.0±0.47ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.17ms     3.8 MB/sec    1.00      4.2±0.19ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   527.0±17.80µs     5.6 MB/sec    1.03   541.9±17.09µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.32ms     3.4 MB/sec    1.00      7.4±0.37ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.08      9.0±0.37ms     4.5 MB/sec    1.00      8.3±0.26ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1878.7±83.77µs     8.9 MB/sec    1.00  1767.9±48.08µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.04    214.4±8.48µs    13.8 MB/sec    1.00    206.6±7.86µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.0±0.16ms     6.4 MB/sec    1.00      3.8±0.12ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.08ms     5.2 MB/sec    1.06      8.2±0.10ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1589.9±24.15µs    10.5 MB/sec    1.04  1661.0±19.99µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    153.6±7.18µs    19.2 MB/sec    1.03    159.0±5.27µs    18.6 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.07ms     8.1 MB/sec    1.06      3.3±0.07ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.26ms     2.5 MB/sec    1.01     16.2±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.02      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.7±8.65µs     6.1 MB/sec    1.03   499.9±11.32µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.01      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.01      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1741.1±17.37µs     9.6 MB/sec    1.02  1774.3±16.19µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.9±3.53µs    15.2 MB/sec    1.04    201.2±3.28µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.02      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-18 10:57_

---

_Label `rule` added by @charliermarsh on 2023-06-18 15:48_

---

_Label `autofix` added by @charliermarsh on 2023-06-18 15:48_

---

_Merged by @charliermarsh on 2023-06-18 15:56_

---

_Closed by @charliermarsh on 2023-06-18 15:56_

---

_Branch deleted on 2023-06-18 15:56_

---
