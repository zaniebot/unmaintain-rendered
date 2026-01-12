```yaml
number: 5793
title: "Use `SmallVec#extend_from_slice` in lieu of `SmallVec#extend`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/extend-from-slice
created_at: 2023-07-16T01:14:28Z
updated_at: 2023-07-16T01:48:13Z
url: https://github.com/astral-sh/ruff/pull/5793
synced_at: 2026-01-12T15:55:19Z
```

# Use `SmallVec#extend_from_slice` in lieu of `SmallVec#extend`

---

_@charliermarsh_

## Summary

There's a note in the docs that suggests this can be faster, and in the benchmarks it... seems like it is? Might just be noise but held up over a few runs.

Before:

<img width="1792" alt="Screen Shot 2023-07-15 at 9 10 06 PM" src="https://github.com/astral-sh/ruff/assets/1309177/973cd955-d4e6-4ae3-898e-90b7eb52ecf2">

After:

<img width="1792" alt="Screen Shot 2023-07-15 at 9 10 09 PM" src="https://github.com/astral-sh/ruff/assets/1309177/1491b391-d219-48e9-aa47-110bc7dc7f90">


---

_Label `internal` added by @charliermarsh on 2023-07-16 01:14_

---

_Label `performance` added by @charliermarsh on 2023-07-16 01:14_

---

_Merged by @charliermarsh on 2023-07-16 01:25_

---

_Closed by @charliermarsh on 2023-07-16 01:25_

---

_Branch deleted on 2023-07-16 01:25_

---

_Comment by @github-actions[bot] on 2023-07-16 01:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.3±0.33ms     3.3 MB/sec    1.01     12.5±0.29ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.5±0.13ms     6.7 MB/sec    1.00      2.4±0.09ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   281.0±20.22µs    10.5 MB/sec    1.00   280.3±10.72µs    10.5 MB/sec
formatter/pydantic/types.py                1.03      5.4±0.22ms     4.7 MB/sec    1.00      5.3±0.17ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.14     21.0±0.60ms  1987.2 KB/sec    1.00     18.3±0.51ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.11      5.1±0.21ms     3.3 MB/sec    1.00      4.6±0.13ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.05   640.5±37.63µs     4.6 MB/sec    1.00   608.4±16.36µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.19      9.8±0.54ms     2.6 MB/sec    1.00      8.2±0.22ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.32     12.0±0.44ms     3.4 MB/sec    1.00      9.1±0.26ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.22      2.4±0.08ms     7.0 MB/sec    1.00  1950.1±55.83µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.11   259.3±12.58µs    11.4 MB/sec    1.00    234.4±7.01µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.26      5.1±0.25ms     5.0 MB/sec    1.00      4.1±0.12ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.2±0.53ms     3.1 MB/sec    1.04     13.7±0.56ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.09ms     6.6 MB/sec    1.05      2.6±0.10ms     6.3 MB/sec
formatter/numpy/globals.py                 1.16  365.2±204.57µs     8.1 MB/sec    1.00   314.7±21.61µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.48ms     4.5 MB/sec    1.06      6.1±0.23ms     4.2 MB/sec
linter/all-rules/large/dataset.py          1.00     19.3±0.74ms     2.1 MB/sec    1.08     20.8±0.75ms  2004.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.60ms     3.2 MB/sec    1.00      5.2±0.27ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.04   631.3±26.33µs     4.7 MB/sec    1.00   605.4±26.93µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.43ms     2.8 MB/sec    1.00      9.1±0.46ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.48ms     4.1 MB/sec    1.05     10.4±0.41ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.12ms     7.7 MB/sec    1.00      2.1±0.11ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.02   266.9±16.16µs    11.1 MB/sec    1.00   262.7±11.19µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.19ms     5.7 MB/sec    1.02      4.6±0.17ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
