```yaml
number: 5769
title: Move lambda visitation into recurse phase
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lambda
created_at: 2023-07-15T02:01:28Z
updated_at: 2023-07-15T02:39:27Z
url: https://github.com/astral-sh/ruff/pull/5769
synced_at: 2026-01-12T03:30:21Z
```

# Move lambda visitation into recurse phase

---

_Pull request opened by @charliermarsh on 2023-07-15 02:01_

## Summary

Similar to #5768: when we analyze a lambda, we need to recurse in the recurse phase, rather than the pre-visit phase.


---

_Label `internal` added by @charliermarsh on 2023-07-15 02:01_

---

_Merged by @charliermarsh on 2023-07-15 02:11_

---

_Closed by @charliermarsh on 2023-07-15 02:11_

---

_Branch deleted on 2023-07-15 02:11_

---

_Comment by @github-actions[bot] on 2023-07-15 02:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.00      9.4±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1848.3±1.83µs     9.0 MB/sec    1.00   1856.0±2.67µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    208.9±0.31µs    14.1 MB/sec    1.00    209.8±0.45µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.00ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.03ms     3.0 MB/sec    1.00     13.4±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    452.8±0.55µs     6.5 MB/sec    1.00    451.6±0.38µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1474.3±4.84µs    11.3 MB/sec    1.00   1470.2±2.02µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.1±0.22µs    17.4 MB/sec    1.00    168.7±0.28µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.2±0.23ms     3.6 MB/sec    1.00     11.1±0.34ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.01      2.2±0.08ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    246.3±5.58µs    12.0 MB/sec    1.01    248.2±7.82µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.09ms     5.4 MB/sec    1.00      4.8±0.15ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.24ms     2.5 MB/sec    1.02     16.4±0.34ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.03      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.8±16.93µs     5.8 MB/sec    1.02   520.4±20.21µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.10ms     3.5 MB/sec    1.03      7.4±0.22ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     5.0 MB/sec    1.01      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1755.2±22.81µs     9.5 MB/sec    1.00  1747.1±31.77µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.03    207.9±5.32µs    14.2 MB/sec    1.00    201.5±3.86µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.07ms     6.9 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
