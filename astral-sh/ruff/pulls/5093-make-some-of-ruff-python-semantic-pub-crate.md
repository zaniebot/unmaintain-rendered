```yaml
number: 5093
title: "Make some of `ruff_python_semantic` `pub(crate)`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/model-visibility
created_at: 2023-06-14T17:40:56Z
updated_at: 2023-06-14T18:10:49Z
url: https://github.com/astral-sh/ruff/pull/5093
synced_at: 2026-01-12T03:43:30Z
```

# Make some of `ruff_python_semantic` `pub(crate)`

---

_Pull request opened by @charliermarsh on 2023-06-14 17:40_

_No description provided._

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-14 17:40_

---

_Label `internal` added by @charliermarsh on 2023-06-14 17:41_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-06-14 17:41_

---

_Merged by @charliermarsh on 2023-06-14 17:49_

---

_Closed by @charliermarsh on 2023-06-14 17:49_

---

_Branch deleted on 2023-06-14 17:49_

---

_Comment by @github-actions[bot] on 2023-06-14 17:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      7.1±0.22ms     5.7 MB/sec    1.00      6.9±0.26ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.04  1525.6±29.59µs    10.9 MB/sec    1.00  1468.2±59.65µs    11.3 MB/sec
formatter/numpy/globals.py                 1.02    141.4±5.80µs    20.9 MB/sec    1.00    138.5±6.26µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.11ms     8.9 MB/sec    1.03      3.0±0.13ms     8.6 MB/sec
linter/all-rules/large/dataset.py          1.02     15.3±0.59ms     2.7 MB/sec    1.00     14.9±0.61ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.12ms     4.6 MB/sec    1.05      3.8±0.13ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.02   467.9±15.16µs     6.3 MB/sec    1.00   459.7±17.02µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.21ms     4.0 MB/sec    1.00      6.3±0.20ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.03      7.5±0.32ms     5.5 MB/sec    1.00      7.2±0.24ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1549.0±47.91µs    10.7 MB/sec    1.01  1559.1±62.56µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.05    185.5±4.50µs    15.9 MB/sec    1.00    176.0±7.06µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.11ms     8.0 MB/sec    1.03      3.3±0.12ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20     10.9±0.20ms     3.7 MB/sec    1.00      9.1±0.14ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.17      2.1±0.04ms     7.8 MB/sec    1.00  1840.1±43.69µs     9.0 MB/sec
formatter/numpy/globals.py                 1.12    197.5±6.71µs    14.9 MB/sec    1.00    176.7±5.82µs    16.7 MB/sec
formatter/pydantic/types.py                1.16      4.3±0.07ms     5.9 MB/sec    1.00      3.7±0.09ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.02     19.7±0.30ms     2.1 MB/sec    1.00     19.3±0.31ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.09ms     3.3 MB/sec    1.00      4.9±0.09ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   592.9±15.56µs     5.0 MB/sec    1.00   586.8±19.95µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.5±0.14ms     3.0 MB/sec    1.00      8.3±0.15ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.9±0.15ms     4.1 MB/sec    1.00      9.8±0.40ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.05ms     7.9 MB/sec    1.00      2.1±0.04ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    237.4±5.96µs    12.4 MB/sec    1.00    230.7±5.36µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.07ms     5.7 MB/sec    1.00      4.4±0.07ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
