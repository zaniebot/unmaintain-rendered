```yaml
number: 5016
title: "Update list of crates in `CONTRIBUTING.md`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/contributing
created_at: 2023-06-11T23:58:23Z
updated_at: 2023-06-12T00:28:32Z
url: https://github.com/astral-sh/ruff/pull/5016
synced_at: 2026-01-12T15:55:17Z
```

# Update list of crates in `CONTRIBUTING.md`

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-11 23:58_

---

_Merged by @charliermarsh on 2023-06-12 00:10_

---

_Closed by @charliermarsh on 2023-06-12 00:10_

---

_Branch deleted on 2023-06-12 00:10_

---

_Comment by @github-actions[bot] on 2023-06-12 00:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      8.0±0.30ms     5.1 MB/sec    1.00      7.7±0.32ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1653.6±65.43µs    10.1 MB/sec    1.00  1629.4±102.45µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00   149.7±10.23µs    19.7 MB/sec    1.03    154.5±8.62µs    19.1 MB/sec
formatter/pydantic/types.py                1.03      3.1±0.22ms     8.2 MB/sec    1.00      3.0±0.18ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.70ms     2.5 MB/sec    1.01     16.3±0.55ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.19ms     4.3 MB/sec    1.00      3.9±0.15ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   492.8±31.12µs     6.0 MB/sec    1.05   518.3±32.30µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.48ms     3.6 MB/sec    1.04      7.2±0.44ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.07      8.4±0.52ms     4.8 MB/sec    1.00      7.9±0.37ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1707.8±97.16µs     9.7 MB/sec    1.00  1702.7±94.42µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.07   219.3±11.99µs    13.5 MB/sec    1.00   204.1±14.29µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.8±0.23ms     6.8 MB/sec    1.00      3.6±0.16ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.07ms     5.3 MB/sec    1.00      7.7±0.10ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1577.4±36.07µs    10.6 MB/sec    1.00  1579.1±25.22µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    151.1±3.20µs    19.5 MB/sec    1.01    152.1±3.39µs    19.4 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.05ms     8.1 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.21ms     2.5 MB/sec    1.00     16.5±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.3±7.56µs     6.0 MB/sec    1.00    495.6±8.25µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.01      7.1±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.08ms     4.9 MB/sec    1.01      8.4±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1763.5±18.68µs     9.4 MB/sec    1.00  1771.4±30.30µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.4±3.37µs    14.7 MB/sec    1.00    201.4±3.21µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.8 MB/sec    1.02      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
