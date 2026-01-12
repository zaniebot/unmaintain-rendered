```yaml
number: 5151
title: Move unconventional import rule to post-binding phase
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-convention
created_at: 2023-06-16T18:02:30Z
updated_at: 2023-06-18T15:39:03Z
url: https://github.com/astral-sh/ruff/pull/5151
synced_at: 2026-01-12T03:43:30Z
```

# Move unconventional import rule to post-binding phase

---

_Pull request opened by @charliermarsh on 2023-06-16 18:02_

## Summary

This PR moves the "unconventional import alias" rule (which enforces, e.g., that `pandas` is imported as `pd`) to the "dead scopes" phase, after the main linter pass. This (1) avoids an allocation since we no longer need to create the qualified name in the linter pass; and (2) will allow us to autofix it, since we'll have access to all references.

## Test Plan

`cargo test` -- all changes are to ranges (which are improvements IMO).


---

_Review requested from @konstin by @charliermarsh on 2023-06-16 18:02_

---

_Comment by @github-actions[bot] on 2023-06-16 18:11_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+8, -8, 0 error(s))

<details><summary>airflow (+8, -8)</summary>
<p>

```diff
+ airflow/executors/celery_executor_utils.py:88:16: ICN001 `numpy` should be imported as `np`
- airflow/executors/celery_executor_utils.py:88:9: ICN001 `numpy` should be imported as `np`
+ airflow/providers/apache/hive/hooks/hive.py:34:12: ICN001 `pandas` should be imported as `pd`
- airflow/providers/apache/hive/hooks/hive.py:34:5: ICN001 `pandas` should be imported as `pd`
+ airflow/providers/oracle/hooks/oracle.py:29:12: ICN001 `numpy` should be imported as `np`
- airflow/providers/oracle/hooks/oracle.py:29:5: ICN001 `numpy` should be imported as `np`
+ airflow/providers/presto/hooks/presto.py:157:16: ICN001 `pandas` should be imported as `pd`
- airflow/providers/presto/hooks/presto.py:157:9: ICN001 `pandas` should be imported as `pd`
+ airflow/providers/trino/hooks/trino.py:172:16: ICN001 `pandas` should be imported as `pd`
- airflow/providers/trino/hooks/trino.py:172:9: ICN001 `pandas` should be imported as `pd`
- tests/providers/oracle/hooks/test_oracle.py:24:1: ICN001 `numpy` should be imported as `np`
+ tests/providers/oracle/hooks/test_oracle.py:24:8: ICN001 `numpy` should be imported as `np`
- tests/serialization/serializers/test_serializers.py:22:1: ICN001 `numpy` should be imported as `np`
+ tests/serialization/serializers/test_serializers.py:22:8: ICN001 `numpy` should be imported as `np`
- tests/serialization/serializers/test_serializers.py:23:1: ICN001 `pandas` should be imported as `pd`
+ tests/serialization/serializers/test_serializers.py:23:8: ICN001 `pandas` should be imported as `pd`
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
formatter/large/dataset.py                 1.00      6.8±0.04ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1393.6±10.51µs    11.9 MB/sec    1.00   1397.3±3.61µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    137.3±1.25µs    21.5 MB/sec    1.01    138.1±1.04µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.05     15.0±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.04      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.2±1.90µs     8.1 MB/sec    1.02    373.7±1.81µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.08ms     4.1 MB/sec    1.03      6.4±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.5 MB/sec    1.11      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1535.4±2.37µs    10.8 MB/sec    1.08   1662.0±4.02µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.4±0.20µs    17.9 MB/sec    1.06    174.4±0.47µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.08      3.6±0.00ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.6±0.07ms     5.3 MB/sec    1.00      7.6±0.05ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1549.4±24.90µs    10.7 MB/sec    1.00  1546.6±27.34µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    149.0±3.60µs    19.8 MB/sec    1.00    149.1±3.88µs    19.8 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.03ms     8.3 MB/sec    1.01      3.1±0.03ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.08ms     2.6 MB/sec    1.00     15.4±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.2±5.79µs     6.1 MB/sec    1.00    486.1±6.27µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.8 MB/sec    1.00      6.8±0.12ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.1 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1708.2±12.91µs     9.7 MB/sec    1.00  1704.8±14.06µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.1±2.97µs    15.2 MB/sec    1.00    195.0±4.29µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/checkers/ast/mod.rs`:4893 on 2023-06-18 11:00_

Should the function containing this still be names `check_dead_scopes`? i feel it has become more of a `check_deferred_scopes`

---

_@konstin approved on 2023-06-18 11:02_

---

_@charliermarsh reviewed on 2023-06-18 15:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4893 on 2023-06-18 15:13_

Going to change this in a separate PR.

---

_Merged by @charliermarsh on 2023-06-18 15:23_

---

_Closed by @charliermarsh on 2023-06-18 15:23_

---

_Branch deleted on 2023-06-18 15:23_

---
