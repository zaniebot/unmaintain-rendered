```yaml
number: 4379
title: Move some recursion out of the pre-visit statement phase
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/arrange
created_at: 2023-05-11T19:35:00Z
updated_at: 2023-05-11T20:03:05Z
url: https://github.com/astral-sh/ruff/pull/4379
synced_at: 2026-01-12T15:55:15Z
```

# Move some recursion out of the pre-visit statement phase

---

_@charliermarsh_

## Summary

These recursive visits are in the wrong "phase", as per our informal multi-phase visiting logic.


---

_Label `autofix` added by @charliermarsh on 2023-05-11 19:35_

---

_Label `autofix` removed by @charliermarsh on 2023-05-11 19:35_

---

_Label `internal` added by @charliermarsh on 2023-05-11 19:35_

---

_Comment by @github-actions[bot] on 2023-05-11 19:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     19.6±0.88ms     2.1 MB/sec    1.00     19.1±0.55ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.6±0.25ms     3.7 MB/sec    1.00      4.5±0.18ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   592.6±61.50µs     5.0 MB/sec    1.01   597.4±40.75µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.40ms     3.2 MB/sec    1.04      8.2±0.35ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.31ms     4.4 MB/sec    1.03      9.5±0.41ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1953.9±81.68µs     8.5 MB/sec    1.03      2.0±0.11ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   225.7±10.78µs    13.1 MB/sec    1.07   241.1±28.81µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.28ms     6.2 MB/sec    1.04      4.3±0.25ms     5.9 MB/sec
parser/large/dataset.py                    1.00      7.3±0.19ms     5.6 MB/sec    1.05      7.6±0.24ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1396.0±31.54µs    11.9 MB/sec    1.04  1446.7±55.80µs    11.5 MB/sec
parser/numpy/globals.py                    1.00    140.2±5.25µs    21.0 MB/sec    1.01    142.2±6.67µs    20.8 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.14ms     8.0 MB/sec    1.00      3.2±0.10ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.4±0.53ms  2046.9 KB/sec    1.00     20.2±0.49ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.2±0.30ms     3.2 MB/sec    1.00      5.1±0.15ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   593.7±15.12µs     5.0 MB/sec    1.00   595.5±18.68µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.6±0.32ms     3.0 MB/sec    1.00      8.5±0.18ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.21ms     4.1 MB/sec    1.00      9.9±0.24ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.9 MB/sec    1.02      2.1±0.11ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   236.8±10.67µs    12.5 MB/sec    1.02   241.9±16.08µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.11ms     5.7 MB/sec    1.01      4.5±0.11ms     5.7 MB/sec
parser/large/dataset.py                    1.00      8.1±0.16ms     5.0 MB/sec    1.00      8.1±0.19ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1539.4±54.75µs    10.8 MB/sec    1.01  1553.5±62.28µs    10.7 MB/sec
parser/numpy/globals.py                    1.00    155.2±4.26µs    19.0 MB/sec    1.01    156.0±4.95µs    18.9 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.08ms     7.4 MB/sec    1.00      3.4±0.10ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-11 19:46_

---

_Closed by @charliermarsh on 2023-05-11 19:46_

---

_Branch deleted on 2023-05-11 19:46_

---
