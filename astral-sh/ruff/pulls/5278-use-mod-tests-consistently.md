```yaml
number: 5278
title: "Use `mod tests` consistently"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tests
created_at: 2023-06-22T01:36:41Z
updated_at: 2023-06-22T02:17:33Z
url: https://github.com/astral-sh/ruff/pull/5278
synced_at: 2026-01-12T03:43:30Z
```

# Use `mod tests` consistently

---

_Pull request opened by @charliermarsh on 2023-06-22 01:36_

As per the Rust documentation.

---

_Label `internal` added by @charliermarsh on 2023-06-22 01:36_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-06-22 01:36_

---

_Review request for @dhruvmanila removed by @charliermarsh on 2023-06-22 01:36_

---

_Merged by @charliermarsh on 2023-06-22 01:50_

---

_Closed by @charliermarsh on 2023-06-22 01:50_

---

_Branch deleted on 2023-06-22 01:50_

---

_Comment by @github-actions[bot] on 2023-06-22 01:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.24      7.7±0.69ms     5.3 MB/sec    1.00      6.2±0.15ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.07  1458.6±99.08µs    11.4 MB/sec    1.00  1367.6±57.83µs    12.2 MB/sec
formatter/numpy/globals.py                 1.00    132.1±3.69µs    22.3 MB/sec    1.02    134.6±8.32µs    21.9 MB/sec
formatter/pydantic/types.py                1.04      2.8±0.09ms     9.1 MB/sec    1.00      2.7±0.10ms     9.5 MB/sec
linter/all-rules/large/dataset.py          1.18     15.4±0.61ms     2.6 MB/sec    1.00     13.1±0.33ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.16      3.9±0.13ms     4.3 MB/sec    1.00      3.3±0.15ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.16   498.3±29.87µs     5.9 MB/sec    1.00   430.6±10.70µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.17      6.9±0.34ms     3.7 MB/sec    1.00      5.8±0.19ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.22      7.8±0.31ms     5.2 MB/sec    1.00      6.4±0.20ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.31  1828.3±91.09µs     9.1 MB/sec    1.00  1395.7±40.51µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.04   175.7±17.90µs    16.8 MB/sec    1.00    168.7±9.10µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.18      3.5±0.25ms     7.3 MB/sec    1.00      2.9±0.08ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.7±0.44ms     3.8 MB/sec    1.05     11.2±0.53ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.13ms     7.6 MB/sec    1.03      2.2±0.18ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   217.0±15.32µs    13.6 MB/sec    1.03   223.4±22.69µs    13.2 MB/sec
formatter/pydantic/types.py                1.05      4.5±0.23ms     5.6 MB/sec    1.00      4.3±0.20ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.05     22.1±1.42ms  1887.7 KB/sec    1.00     21.0±0.92ms  1987.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.5±0.23ms     3.0 MB/sec    1.00      5.4±0.30ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   671.9±44.45µs     4.4 MB/sec    1.00   664.9±43.19µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.49ms     2.8 MB/sec    1.00      9.3±0.54ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.03     10.9±0.52ms     3.7 MB/sec    1.00     10.5±0.50ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.4 MB/sec    1.00      2.2±0.13ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   277.6±28.17µs    10.6 MB/sec    1.02   283.7±20.19µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.9±0.21ms     5.2 MB/sec    1.00      4.9±0.30ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
