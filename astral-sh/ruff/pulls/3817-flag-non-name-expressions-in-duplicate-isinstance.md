```yaml
number: 3817
title: "Flag non-`Name` expressions in `duplicate-isinstance-call`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/sim101
created_at: 2023-03-30T16:05:56Z
updated_at: 2023-03-30T16:38:45Z
url: https://github.com/astral-sh/ruff/pull/3817
synced_at: 2026-01-12T04:28:19Z
```

# Flag non-`Name` expressions in `duplicate-isinstance-call`

---

_Pull request opened by @charliermarsh on 2023-03-30 16:05_

Closes #3815.

---

_Renamed from "Flag non-Name expressions in duplicate-isinstance-call" to "Flag non-`Name` expressions in `duplicate-isinstance-call`" by @charliermarsh on 2023-03-30 16:06_

---

_Comment by @github-actions[bot] on 2023-03-30 16:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.00     13.8±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    456.8±0.74µs     6.5 MB/sec    1.01    461.2±0.75µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.01      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.6 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1625.9±3.17µs    10.2 MB/sec    1.00   1631.1±2.30µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.1±0.39µs    16.4 MB/sec    1.01    182.5±0.54µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     22.6±0.93ms  1843.5 KB/sec    1.04     23.6±0.83ms  1768.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.32ms     2.9 MB/sec    1.03      5.9±0.30ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   669.3±38.08µs     4.4 MB/sec    1.09   730.0±35.72µs     4.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.4±0.35ms     2.7 MB/sec    1.06     10.0±0.50ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.1±0.43ms     3.7 MB/sec    1.10     12.1±0.41ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.14ms     6.7 MB/sec    1.04      2.6±0.12ms     6.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   288.0±32.65µs    10.2 MB/sec    1.08   311.8±23.71µs     9.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.2±0.20ms     4.9 MB/sec    1.07      5.6±0.24ms     4.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-30 16:19_

---

_Closed by @charliermarsh on 2023-03-30 16:19_

---

_Branch deleted on 2023-03-30 16:19_

---
