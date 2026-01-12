```yaml
number: 4026
title: remove unnecessary f-string formatting
type: pull_request
state: merged
author: Xemnas0
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2023-04-19T18:08:59Z
updated_at: 2023-04-19T18:38:15Z
url: https://github.com/astral-sh/ruff/pull/4026
synced_at: 2026-01-12T15:55:14Z
```

# remove unnecessary f-string formatting

---

_@Xemnas0_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-19 18:14_

---

_Closed by @charliermarsh on 2023-04-19 18:14_

---

_Comment by @github-actions[bot] on 2023-04-19 18:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.11ms     2.8 MB/sec    1.00     14.6±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.6±1.20µs     6.4 MB/sec    1.00    457.6±1.77µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1611.3±5.42µs    10.3 MB/sec    1.01   1625.9±3.23µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.2±0.41µs    16.4 MB/sec    1.00    179.3±0.58µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.04      6.1±0.01ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1155.0±0.62µs    14.4 MB/sec    1.03   1188.8±4.26µs    14.0 MB/sec
parser/numpy/globals.py                    1.00    114.9±0.31µs    25.7 MB/sec    1.01    116.1±0.44µs    25.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.03      2.6±0.01ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.14ms     2.6 MB/sec    1.00     15.8±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.0 MB/sec    1.00      4.1±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.7±8.07µs     5.9 MB/sec    1.01   503.8±32.87µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.8 MB/sec    1.00      6.8±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     5.0 MB/sec    1.00      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1801.5±21.40µs     9.2 MB/sec    1.00  1803.9±15.06µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.6±2.69µs    15.1 MB/sec    1.01    197.3±4.27µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.5±0.06ms     6.3 MB/sec    1.01      6.5±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1225.7±12.48µs    13.6 MB/sec    1.01  1240.0±16.93µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    123.0±2.33µs    24.0 MB/sec    1.01    124.8±2.33µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.3 MB/sec    1.01      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-19 18:26_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2023-04-19 18:27_

---
