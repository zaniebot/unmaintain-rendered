```yaml
number: 5053
title: "Use `.is_unbound()` in flake8-errmsg fix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/binding
created_at: 2023-06-13T16:12:41Z
updated_at: 2023-06-13T16:46:58Z
url: https://github.com/astral-sh/ruff/pull/5053
synced_at: 2026-01-12T15:55:17Z
```

# Use `.is_unbound()` in flake8-errmsg fix

---

_@charliermarsh_

## Summary

Trying to bring some more consistent to these APIs as I look to change them to accommodate deletions.

---

_Label `internal` added by @charliermarsh on 2023-06-13 16:12_

---

_Merged by @charliermarsh on 2023-06-13 16:22_

---

_Closed by @charliermarsh on 2023-06-13 16:22_

---

_Branch deleted on 2023-06-13 16:22_

---

_Comment by @github-actions[bot] on 2023-06-13 16:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.24ms     6.4 MB/sec    1.01      6.4±0.22ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1345.9±61.49µs    12.4 MB/sec    1.01  1356.3±68.51µs    12.3 MB/sec
formatter/numpy/globals.py                 1.07    139.1±7.50µs    21.2 MB/sec    1.00    130.2±8.22µs    22.7 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.13ms     9.7 MB/sec    1.00      2.6±0.11ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.55ms     2.9 MB/sec    1.00     14.1±0.51ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.14ms     4.7 MB/sec    1.00      3.4±0.14ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   430.5±19.37µs     6.9 MB/sec    1.02   440.6±22.39µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.25ms     4.2 MB/sec    1.00      6.0±0.18ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.04      7.1±0.29ms     5.7 MB/sec    1.00      6.8±0.25ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1514.0±60.13µs    11.0 MB/sec    1.00  1496.3±57.31µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    172.4±7.53µs    17.1 MB/sec    1.00    168.1±8.23µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.09ms     8.2 MB/sec    1.00      3.1±0.13ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.7±0.07ms     5.3 MB/sec    1.00      7.6±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1579.6±31.42µs    10.5 MB/sec    1.00  1559.2±22.96µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    150.7±3.96µs    19.6 MB/sec    1.00    150.8±4.96µs    19.6 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.1 MB/sec    1.00      3.1±0.06ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.25ms     2.5 MB/sec    1.00     16.5±0.27ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.09ms     4.0 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    499.6±6.30µs     5.9 MB/sec    1.00    491.2±6.99µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.12ms     3.6 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.09ms     4.9 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1755.3±26.13µs     9.5 MB/sec    1.00  1741.4±21.38µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.2±4.62µs    15.0 MB/sec    1.00    196.1±3.41µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
