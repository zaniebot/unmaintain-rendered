```yaml
number: 6529
title: "Add docs for `DTZ005` and `DTZ006`"
type: pull_request
state: merged
author: klistwan
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/DTZ005-DTZ006
created_at: 2023-08-13T00:36:38Z
updated_at: 2023-08-13T01:29:37Z
url: https://github.com/astral-sh/ruff/pull/6529
synced_at: 2026-01-12T15:55:21Z
```

# Add docs for `DTZ005` and `DTZ006`

---

_@klistwan_

Changes:
- Adds docs for `DTZ005`
- Adds docs for `DTZ006`

Related to: https://github.com/astral-sh/ruff/issues/2646

---

_Comment by @github-actions[bot] on 2023-08-13 00:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.4±0.13ms     5.5 MB/sec    1.02      7.6±0.27ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1483.2±51.41µs    11.2 MB/sec    1.01  1497.0±53.78µs    11.1 MB/sec
formatter/numpy/globals.py                 1.00    168.6±8.98µs    17.5 MB/sec    1.01    169.6±8.74µs    17.4 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.13ms     8.1 MB/sec    1.00      3.1±0.11ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.01      9.5±0.29ms     4.3 MB/sec    1.00      9.4±0.22ms     4.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.6±0.10ms     6.5 MB/sec    1.00      2.6±0.07ms     6.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   367.0±14.03µs     8.0 MB/sec    1.01   369.8±15.43µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.13ms     5.2 MB/sec    1.01      5.0±0.16ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.0±0.09ms     8.2 MB/sec    1.00      5.0±0.10ms     8.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1106.3±45.28µs    15.1 MB/sec    1.00  1102.7±33.83µs    15.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    127.4±4.97µs    23.2 MB/sec    1.00    126.8±3.81µs    23.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.2±0.06ms    11.4 MB/sec    1.00      2.2±0.05ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.13ms     4.1 MB/sec    1.02     10.2±0.13ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1924.9±24.72µs     8.7 MB/sec    1.02  1962.4±27.87µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    218.6±8.61µs    13.5 MB/sec    1.01    220.0±5.62µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.07ms     6.1 MB/sec    1.03      4.3±0.08ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.12ms     3.2 MB/sec    1.00     12.8±0.12ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.01      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    442.9±7.38µs     6.7 MB/sec    1.01    445.7±7.25µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.7±0.12ms     3.8 MB/sec    1.00      6.6±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.09ms     5.9 MB/sec    1.00      6.9±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1491.0±49.07µs    11.2 MB/sec    1.00  1488.2±32.61µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.8±2.90µs    16.9 MB/sec    1.01    176.0±3.93µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-13 01:29_

---

_Merged by @charliermarsh on 2023-08-13 01:29_

---

_Closed by @charliermarsh on 2023-08-13 01:29_

---

_Comment by @charliermarsh on 2023-08-13 01:29_

Thanks!

---
