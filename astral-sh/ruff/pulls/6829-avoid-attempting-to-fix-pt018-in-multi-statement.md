```yaml
number: 6829
title: Avoid attempting to fix PT018 in multi-statement lines
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/PT018
created_at: 2023-08-23T22:53:22Z
updated_at: 2023-08-23T23:32:11Z
url: https://github.com/astral-sh/ruff/pull/6829
synced_at: 2026-01-12T15:55:22Z
```

# Avoid attempting to fix PT018 in multi-statement lines

---

_@charliermarsh_

## Summary

These fixes will _always_ fail, so we should avoid trying to construct them in the first place.

Closes https://github.com/astral-sh/ruff/issues/6812.


---

_Label `bug` added by @charliermarsh on 2023-08-23 22:54_

---

_Merged by @charliermarsh on 2023-08-23 23:09_

---

_Closed by @charliermarsh on 2023-08-23 23:09_

---

_Branch deleted on 2023-08-23 23:09_

---

_Comment by @github-actions[bot] on 2023-08-23 23:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.5±0.02ms    11.5 MB/sec    1.00      3.5±0.03ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.00    709.8±6.62µs    23.5 MB/sec    1.00   710.1±11.99µs    23.4 MB/sec
formatter/numpy/globals.py                 1.00     73.9±0.35µs    39.9 MB/sec    1.01     74.4±0.93µs    39.7 MB/sec
formatter/pydantic/types.py                1.00  1420.4±12.02µs    18.0 MB/sec    1.01  1430.9±21.28µs    17.8 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.01ms     3.9 MB/sec    1.01     10.4±0.01ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.00ms     5.9 MB/sec    1.01      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    316.8±1.15µs     9.3 MB/sec    1.00    317.5±1.57µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.00      5.4±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.4 MB/sec    1.02      5.6±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1173.1±5.16µs    14.2 MB/sec    1.01   1184.1±2.00µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.3±0.32µs    23.7 MB/sec    1.00    124.4±0.32µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.3±0.34ms     7.7 MB/sec    1.02      5.4±0.24ms     7.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1045.1±61.71µs    15.9 MB/sec    1.01  1054.9±41.16µs    15.8 MB/sec
formatter/numpy/globals.py                 1.00    103.4±7.98µs    28.6 MB/sec    1.02    105.5±6.50µs    28.0 MB/sec
formatter/pydantic/types.py                1.00      2.1±0.08ms    12.0 MB/sec    1.02      2.2±0.10ms    11.8 MB/sec
linter/all-rules/large/dataset.py          1.02     17.8±0.62ms     2.3 MB/sec    1.00     17.4±0.52ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.8±0.21ms     3.5 MB/sec    1.00      4.7±0.16ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.04   612.5±31.48µs     4.8 MB/sec    1.00   589.1±19.98µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.2±0.31ms     2.8 MB/sec    1.00      8.9±0.24ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.32ms     4.2 MB/sec    1.04     10.0±0.61ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.06ms     8.2 MB/sec    1.07      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   246.6±12.13µs    12.0 MB/sec    1.06   261.9±17.93µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.17ms     6.0 MB/sec    1.06      4.5±0.20ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
