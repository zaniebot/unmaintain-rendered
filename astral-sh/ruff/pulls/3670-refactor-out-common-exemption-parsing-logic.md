```yaml
number: 3670
title: Refactor out common exemption-parsing logic
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/exemption
created_at: 2023-03-22T18:08:41Z
updated_at: 2023-03-22T19:02:09Z
url: https://github.com/astral-sh/ruff/pull/3670
synced_at: 2026-01-12T15:55:13Z
```

# Refactor out common exemption-parsing logic

---

_@charliermarsh_

## Summary

This is a non-behavior-changing refactor that I initially pursued as part of https://github.com/charliermarsh/ruff/pull/3665. I ended up using a different solution for that PR, but this feels like an improvement anyway.


---

_Label `internal` added by @charliermarsh on 2023-03-22 18:08_

---

_Comment by @github-actions[bot] on 2023-03-22 18:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.04ms     2.9 MB/sec    1.01     14.3±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.01      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    437.3±1.59µs     6.7 MB/sec    1.00    432.6±1.56µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1716.3±3.82µs     9.7 MB/sec    1.00   1717.2±4.05µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.7±0.30µs    16.7 MB/sec    1.00    176.9±0.96µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.23ms     2.5 MB/sec    1.01     16.4±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.11ms     3.8 MB/sec    1.01      4.4±0.15ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   546.7±10.34µs     5.4 MB/sec    1.02   556.1±13.73µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.10ms     3.5 MB/sec    1.00      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.12ms     4.7 MB/sec    1.01      8.7±0.11ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1881.1±19.00µs     8.9 MB/sec    1.01  1893.7±18.37µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.5±6.79µs    14.4 MB/sec    1.01    206.3±6.69µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.10ms     6.3 MB/sec    1.00      4.0±0.07ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-22 19:02_

---

_Closed by @charliermarsh on 2023-03-22 19:02_

---

_Branch deleted on 2023-03-22 19:02_

---
