```yaml
number: 5501
title: "[`pylint`] Implement Pylint `typevar-name-mismatch` (`C0132`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: typevar-pylint-rules
created_at: 2023-07-04T13:25:14Z
updated_at: 2023-07-10T09:54:56Z
url: https://github.com/astral-sh/ruff/pull/5501
synced_at: 2026-01-12T15:55:18Z
```

# [`pylint`] Implement Pylint `typevar-name-mismatch` (`C0132`)

---

_@tjkuson_

## Summary

Implement Pylint `typevar-name-mismatch` (`C0132`) as `type-param-name-mismatch` (`PLC0132`). Includes documentation. Related to #970.

The Pylint implementation checks only `TypeVar`, but this PR checks `TypeVarTuple`, `ParamSpec`, and `NewType` as well. This seems to better represent the Pylint rule's [intended behaviour](https://github.com/pylint-dev/pylint/issues/5224).

Full disclosure: I am not a fan of the translated name and think it should probably be different.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-04 13:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.7±0.46ms     3.8 MB/sec    1.00     10.5±0.27ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.3±0.13ms     7.1 MB/sec    1.00      2.3±0.12ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   260.2±21.91µs    11.3 MB/sec    1.05   272.1±31.14µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.24ms     5.2 MB/sec    1.03      5.1±0.25ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     19.1±0.76ms     2.1 MB/sec    1.00     19.0±0.82ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.7±0.54ms     3.6 MB/sec    1.00      4.6±0.23ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   580.0±24.58µs     5.1 MB/sec    1.04   605.7±71.81µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.26ms     3.2 MB/sec    1.06      8.5±0.41ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03      9.3±0.40ms     4.4 MB/sec    1.00      9.0±0.29ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1936.5±82.54µs     8.6 MB/sec    1.02  1977.6±60.86µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.03   235.0±19.92µs    12.6 MB/sec    1.00    228.1±9.53µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.4±0.23ms     5.8 MB/sec    1.00      4.1±0.21ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.2±0.06ms     4.4 MB/sec    1.00      9.2±0.07ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.2 MB/sec    1.00      2.0±0.03ms     8.2 MB/sec
formatter/numpy/globals.py                 1.01    234.1±6.61µs    12.6 MB/sec    1.00    232.5±5.97µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.05ms     5.8 MB/sec    1.00      4.4±0.06ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.11ms     2.6 MB/sec    1.00     15.4±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   519.1±11.51µs     5.7 MB/sec    1.00    500.5±8.23µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.09ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.08ms     5.0 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1727.1±13.53µs     9.6 MB/sec    1.00  1700.2±19.55µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.04    207.6±4.17µs    14.2 MB/sec    1.00    200.6±2.84µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.11ms     6.9 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-04 18:18_

---

_Label `rule` added by @charliermarsh on 2023-07-04 18:18_

---

_Renamed from "Implement Pylint `typevar-name-mismatch` (`C0132`)" to "[`pylint`] Implement Pylint `typevar-name-mismatch` (`C0132`)" by @charliermarsh on 2023-07-04 18:44_

---

_Merged by @charliermarsh on 2023-07-04 18:49_

---

_Closed by @charliermarsh on 2023-07-04 18:49_

---

_Branch deleted on 2023-07-10 09:54_

---
