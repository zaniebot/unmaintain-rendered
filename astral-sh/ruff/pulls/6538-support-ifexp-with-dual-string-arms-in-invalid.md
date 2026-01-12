```yaml
number: 6538
title: "Support `IfExp` with dual string arms in `invalid-envvar-value`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/if-exp
created_at: 2023-08-13T18:32:19Z
updated_at: 2023-08-13T19:52:11Z
url: https://github.com/astral-sh/ruff/pull/6538
synced_at: 2026-01-12T15:55:21Z
```

# Support `IfExp` with dual string arms in `invalid-envvar-value`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/6537. We need to improve the `PythonType` algorithm, so this also documents some of its limitations as TODOs.


---

_Label `bug` added by @charliermarsh on 2023-08-13 18:32_

---

_Comment by @github-actions[bot] on 2023-08-13 18:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.04ms     4.0 MB/sec    1.01     10.2±0.06ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.2 MB/sec    1.00      2.0±0.02ms     8.2 MB/sec
formatter/numpy/globals.py                 1.01    232.9±7.47µs    12.7 MB/sec    1.00    230.4±5.80µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.07ms     5.9 MB/sec    1.00      4.3±0.09ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.03ms     3.2 MB/sec    1.00     12.8±0.02ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.1±0.89µs     6.1 MB/sec    1.01    487.7±3.16µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.02ms     3.9 MB/sec    1.00      6.6±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.01      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1489.3±2.88µs    11.2 MB/sec    1.01   1500.3±7.20µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.1±1.40µs    16.8 MB/sec    1.00    175.7±0.30µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.04ms     4.1 MB/sec    1.02     10.1±0.08ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1910.8±30.68µs     8.7 MB/sec    1.01  1938.8±11.12µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    195.7±3.82µs    15.1 MB/sec    1.03    201.2±7.30µs    14.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.0 MB/sec    1.02      4.3±0.04ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.06ms     3.2 MB/sec    1.00     12.7±0.06ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.4±5.55µs     7.9 MB/sec    1.00    373.3±3.58µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.9 MB/sec    1.01      6.6±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.03ms     5.9 MB/sec    1.00      6.9±0.08ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1457.5±9.51µs    11.4 MB/sec    1.00  1460.4±17.21µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.8±2.00µs    19.7 MB/sec    1.02    152.2±2.89µs    19.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-13 19:52_

---

_Closed by @charliermarsh on 2023-08-13 19:52_

---

_Branch deleted on 2023-08-13 19:52_

---
