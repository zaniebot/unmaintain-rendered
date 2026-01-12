```yaml
number: 4772
title: "Use `ALL` in `fixable` documentation"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fix-all
created_at: 2023-06-01T02:16:10Z
updated_at: 2023-06-01T02:51:35Z
url: https://github.com/astral-sh/ruff/pull/4772
synced_at: 2026-01-12T15:55:16Z
```

# Use `ALL` in `fixable` documentation

---

_@charliermarsh_

It's confusing to enumerate all codes here, and often gets out-of-date.

---

_Renamed from "Use ALL in --fixable documentation" to "Use `ALL` in `fixable` documentation" by @charliermarsh on 2023-06-01 02:16_

---

_Comment by @github-actions[bot] on 2023-06-01 02:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.4±0.15ms     2.8 MB/sec    1.00     14.2±0.14ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    424.7±0.65µs     6.9 MB/sec    1.00    421.5±0.58µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.06ms     4.3 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.03ms     5.9 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1501.3±5.46µs    11.1 MB/sec    1.00   1500.8±3.67µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.2±0.29µs    17.3 MB/sec    1.00    171.0±2.13µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.01      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.02ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.01   1023.7±3.11µs    16.3 MB/sec    1.00   1014.0±0.66µs    16.4 MB/sec
parser/numpy/globals.py                    1.01    105.8±0.16µs    27.9 MB/sec    1.00    104.6±0.26µs    28.2 MB/sec
parser/pydantic/types.py                   1.01      2.2±0.00ms    11.3 MB/sec    1.00      2.2±0.01ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.1±1.29ms     2.0 MB/sec    1.01     20.4±1.10ms  2039.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.28ms     3.3 MB/sec    1.00      5.0±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.04   614.5±57.98µs     4.8 MB/sec    1.00   592.1±35.67µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.33ms     3.2 MB/sec    1.08      8.7±0.43ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.47ms     4.2 MB/sec    1.02      9.8±0.47ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1988.7±88.63µs     8.4 MB/sec    1.08      2.2±0.12ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.04   252.0±17.50µs    11.7 MB/sec    1.00   241.8±13.77µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.31ms     5.6 MB/sec    1.00      4.5±0.26ms     5.7 MB/sec
parser/large/dataset.py                    1.00      7.6±0.28ms     5.3 MB/sec    1.02      7.8±0.31ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1498.6±80.69µs    11.1 MB/sec    1.01  1517.6±86.71µs    11.0 MB/sec
parser/numpy/globals.py                    1.00    150.2±8.97µs    19.6 MB/sec    1.04   156.2±13.43µs    18.9 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.17ms     7.6 MB/sec    1.03      3.4±0.15ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-01 02:30_

---

_Closed by @charliermarsh on 2023-06-01 02:30_

---

_Branch deleted on 2023-06-01 02:30_

---
