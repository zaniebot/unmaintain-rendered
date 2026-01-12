```yaml
number: 6042
title: Move comprehension rules into shared analyze method
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comprehension
created_at: 2023-07-24T21:09:37Z
updated_at: 2023-07-24T21:42:11Z
url: https://github.com/astral-sh/ruff/pull/6042
synced_at: 2026-01-12T03:30:22Z
```

# Move comprehension rules into shared analyze method

---

_Pull request opened by @charliermarsh on 2023-07-24 21:09_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-07-24 21:14_

---

_Merged by @charliermarsh on 2023-07-24 21:18_

---

_Closed by @charliermarsh on 2023-07-24 21:18_

---

_Branch deleted on 2023-07-24 21:18_

---

_Comment by @github-actions[bot] on 2023-07-24 21:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     10.3±0.36ms     4.0 MB/sec    1.00      9.8±0.35ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.09ms     8.2 MB/sec    1.00      2.0±0.10ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   232.2±14.12µs    12.7 MB/sec    1.07    249.6±9.85µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.19ms     5.8 MB/sec    1.05      4.6±0.17ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.67ms     2.9 MB/sec    1.01     14.1±0.56ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.7±0.16ms     4.5 MB/sec    1.00      3.5±0.15ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   485.5±25.05µs     6.1 MB/sec    1.00   481.0±15.71µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.32ms     4.0 MB/sec    1.02      6.5±0.20ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.08      8.0±0.06ms     5.1 MB/sec    1.00      7.4±0.32ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1696.7±28.44µs     9.8 MB/sec    1.00  1579.5±57.68µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±6.88µs    16.4 MB/sec    1.02    184.4±4.84µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.6±0.05ms     7.1 MB/sec    1.00      3.4±0.11ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     14.0±0.49ms     2.9 MB/sec    1.00     13.7±0.47ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.24ms     6.1 MB/sec    1.00      2.7±0.11ms     6.1 MB/sec
formatter/numpy/globals.py                 1.00   310.7±20.06µs     9.5 MB/sec    1.01   315.3±21.11µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      5.9±0.25ms     4.3 MB/sec    1.02      6.0±0.25ms     4.2 MB/sec
linter/all-rules/large/dataset.py          1.00     18.9±0.67ms     2.1 MB/sec    1.01     19.1±0.50ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.19ms     3.4 MB/sec    1.01      5.0±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   605.3±39.73µs     4.9 MB/sec    1.00   601.3±27.52µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.33ms     3.0 MB/sec    1.02      8.8±0.35ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.32ms     4.0 MB/sec    1.00     10.2±0.34ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.09ms     7.7 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.03   245.0±15.18µs    12.0 MB/sec    1.00   237.3±11.92µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.18ms     5.6 MB/sec    1.00      4.5±0.17ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
