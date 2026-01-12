```yaml
number: 6688
title: "Add docs for `DTZ011` and `DTZ012`"
type: pull_request
state: merged
author: klistwan
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/DTZ011-DTZ012
created_at: 2023-08-19T02:46:06Z
updated_at: 2023-08-22T07:16:20Z
url: https://github.com/astral-sh/ruff/pull/6688
synced_at: 2026-01-12T15:55:22Z
```

# Add docs for `DTZ011` and `DTZ012`

---

_@klistwan_

Changes:
- Adds docs for `DTZ011`
- Adds docs for `DTZ012`

Related to: https://github.com/astral-sh/ruff/issues/2646

---

_Renamed from "Add docs for DTZ011 and DTZ012" to "Add docs for `DTZ011` and `DTZ012`" by @klistwan on 2023-08-19 02:46_

---

_Comment by @github-actions[bot] on 2023-08-19 03:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.2±0.02ms    12.7 MB/sec    1.01      3.2±0.01ms    12.6 MB/sec
formatter/numpy/ctypeslib.py               1.00    660.9±7.46µs    25.2 MB/sec    1.02   670.9±12.13µs    24.8 MB/sec
formatter/numpy/globals.py                 1.00     69.1±0.44µs    42.7 MB/sec    1.01     69.8±0.33µs    42.3 MB/sec
formatter/pydantic/types.py                1.00  1317.1±14.45µs    19.4 MB/sec    1.01  1324.3±27.13µs    19.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.02ms     3.9 MB/sec    1.00     10.5±0.01ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    331.8±1.26µs     8.9 MB/sec    1.00    328.7±0.71µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.01ms     4.7 MB/sec    1.00      5.5±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      5.6±0.01ms     7.3 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1187.6±2.59µs    14.0 MB/sec    1.00   1190.4±2.47µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    122.7±0.42µs    24.0 MB/sec    1.00    123.3±0.23µs    23.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.06ms    10.9 MB/sec    1.00      3.7±0.08ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   766.6±22.31µs    21.7 MB/sec    1.00   767.2±17.51µs    21.7 MB/sec
formatter/numpy/globals.py                 1.01     80.2±3.30µs    36.8 MB/sec    1.00     79.6±2.62µs    37.1 MB/sec
formatter/pydantic/types.py                1.00  1541.2±26.81µs    16.5 MB/sec    1.01  1550.8±33.11µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.21ms     3.2 MB/sec    1.00     12.9±0.14ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.01      3.5±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.2±6.33µs     6.7 MB/sec    1.02   448.1±15.09µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.10ms     3.8 MB/sec    1.00      6.7±0.10ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.06ms     5.8 MB/sec    1.01      7.1±0.07ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1500.2±31.63µs    11.1 MB/sec    1.01  1512.9±25.09µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.5±3.47µs    16.8 MB/sec    1.00    176.3±4.85µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.1 MB/sec    1.00      3.2±0.03ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-20 14:21_

---

_Merged by @charliermarsh on 2023-08-20 14:21_

---

_Closed by @charliermarsh on 2023-08-20 14:21_

---

_Branch deleted on 2023-08-22 07:16_

---
