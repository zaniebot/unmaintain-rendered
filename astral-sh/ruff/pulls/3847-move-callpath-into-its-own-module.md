```yaml
number: 3847
title: "Move `CallPath` into its own module"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/call-path
created_at: 2023-04-01T15:12:28Z
updated_at: 2023-04-01T15:40:24Z
url: https://github.com/astral-sh/ruff/pull/3847
synced_at: 2026-01-12T04:28:19Z
```

# Move `CallPath` into its own module

---

_Pull request opened by @charliermarsh on 2023-04-01 15:12_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-01 15:25_

---

_Closed by @charliermarsh on 2023-04-01 15:25_

---

_Branch deleted on 2023-04-01 15:25_

---

_Comment by @github-actions[bot] on 2023-04-01 15:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     16.9±1.60ms     2.4 MB/sec     1.07     18.1±2.19ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.08      4.4±0.29ms     3.8 MB/sec     1.00      4.1±0.45ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   553.9±35.39µs     5.3 MB/sec     1.00   549.5±46.65µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.4±0.56ms     3.4 MB/sec     1.00      7.3±0.76ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.67ms     4.6 MB/sec     1.04      9.1±0.60ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1988.0±141.23µs     8.4 MB/sec    1.00  1973.3±140.29µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.0±14.09µs    12.4 MB/sec     1.00   237.6±13.73µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.2±0.42ms     6.1 MB/sec     1.00      4.1±0.34ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.3±0.70ms     2.0 MB/sec    1.00     20.1±0.82ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.25ms     3.3 MB/sec    1.00      5.1±0.27ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   606.2±27.31µs     4.9 MB/sec    1.02   620.3±55.83µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.30ms     3.1 MB/sec    1.01      8.4±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02     10.4±0.44ms     3.9 MB/sec    1.00     10.2±0.27ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.5 MB/sec    1.02      2.3±0.10ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.1±16.47µs    12.1 MB/sec    1.05   255.3±19.25µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.21ms     5.5 MB/sec    1.02      4.7±0.22ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
