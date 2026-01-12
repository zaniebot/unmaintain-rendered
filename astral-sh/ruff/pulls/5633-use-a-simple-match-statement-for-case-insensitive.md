```yaml
number: 5633
title: Use a simple match statement for case-insensitive noqa lookup
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - suppression
assignees: []
merged: true
base: main
head: charlie/matches
created_at: 2023-07-10T02:03:20Z
updated_at: 2023-07-10T02:41:38Z
url: https://github.com/astral-sh/ruff/pull/5633
synced_at: 2026-01-12T15:55:19Z
```

# Use a simple match statement for case-insensitive noqa lookup

---

_@charliermarsh_

## Summary

It turns out that just doing this match directly without `AhoCorasick` is much faster, like 2x (and removes one dependency, though we likely already rely on this transitively).


---

_Merged by @charliermarsh on 2023-07-10 02:15_

---

_Closed by @charliermarsh on 2023-07-10 02:15_

---

_Branch deleted on 2023-07-10 02:15_

---

_Label `performance` added by @charliermarsh on 2023-07-10 02:15_

---

_Label `noqa` added by @charliermarsh on 2023-07-10 02:15_

---

_Comment by @github-actions[bot] on 2023-07-10 02:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.02ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1775.2±2.22µs     9.4 MB/sec    1.00   1776.7±2.74µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    194.6±0.53µs    15.2 MB/sec    1.01    195.8±0.41µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.05ms     2.9 MB/sec    1.00     13.9±0.01ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.9±0.92µs     8.1 MB/sec    1.00    362.5±1.22µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.03ms     4.1 MB/sec    1.00      6.2±0.00ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.03ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1470.6±3.47µs    11.3 MB/sec    1.00   1468.9±1.81µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.7±0.24µs    18.8 MB/sec    1.00    156.6±0.44µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.12ms     4.4 MB/sec    1.01      9.4±0.15ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.2 MB/sec    1.01      2.1±0.04ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    233.7±4.88µs    12.6 MB/sec    1.02    238.8±6.78µs    12.4 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.06ms     5.6 MB/sec    1.04      4.7±0.10ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.15ms     2.6 MB/sec    1.02     16.0±0.34ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   512.9±20.40µs     5.8 MB/sec    1.01   517.4±16.86µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.00      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.00      8.0±0.09ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1733.1±23.74µs     9.6 MB/sec    1.01  1742.3±35.25µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.7±5.76µs    14.3 MB/sec    1.00    206.5±4.59µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.01      3.7±0.06ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
