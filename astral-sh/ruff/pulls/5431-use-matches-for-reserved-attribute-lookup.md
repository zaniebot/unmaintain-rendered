```yaml
number: 5431
title: "Use `matches!` for reserved attribute lookup"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/attr
created_at: 2023-06-29T01:42:50Z
updated_at: 2023-06-29T02:18:41Z
url: https://github.com/astral-sh/ruff/pull/5431
synced_at: 2026-01-12T03:36:55Z
```

# Use `matches!` for reserved attribute lookup

---

_Pull request opened by @charliermarsh on 2023-06-29 01:42_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-29 01:42_

---

_Merged by @charliermarsh on 2023-06-29 01:52_

---

_Closed by @charliermarsh on 2023-06-29 01:52_

---

_Branch deleted on 2023-06-29 01:52_

---

_Comment by @github-actions[bot] on 2023-06-29 01:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.05      9.1±0.53ms     4.5 MB/sec     1.00      8.7±0.56ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.07  1901.8±121.33µs     8.8 MB/sec    1.00  1785.4±123.49µs     9.3 MB/sec
formatter/numpy/globals.py                 1.12   226.8±14.63µs    13.0 MB/sec     1.00   202.7±11.93µs    14.6 MB/sec
formatter/pydantic/types.py                1.17      4.3±0.23ms     5.9 MB/sec     1.00      3.7±0.20ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.03     16.3±0.76ms     2.5 MB/sec     1.00     15.8±1.08ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.18ms     4.3 MB/sec     1.02      3.9±0.28ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   484.3±36.63µs     6.1 MB/sec     1.07   518.7±24.14µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.37ms     3.8 MB/sec     1.05      7.1±0.26ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.45ms     5.3 MB/sec     1.00      7.7±0.42ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1669.7±86.36µs    10.0 MB/sec     1.02  1702.3±69.03µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.12   217.1±11.34µs    13.6 MB/sec     1.00   193.1±16.63µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.7±0.16ms     6.9 MB/sec     1.00      3.5±0.20ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.4±0.48ms     3.3 MB/sec    1.02     12.7±0.42ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.14ms     6.4 MB/sec    1.01      2.6±0.09ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   293.4±17.80µs    10.1 MB/sec    1.03   301.4±18.58µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.6±0.20ms     4.6 MB/sec    1.03      5.8±0.24ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.11     22.6±1.12ms  1844.1 KB/sec    1.00     20.4±0.90ms  2042.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.7±0.22ms     2.9 MB/sec    1.00      5.4±0.21ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   673.4±37.60µs     4.4 MB/sec    1.00   655.8±29.70µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.07      9.7±0.45ms     2.6 MB/sec    1.00      9.1±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.29ms     3.7 MB/sec    1.04     11.4±0.52ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.10ms     7.5 MB/sec    1.00      2.2±0.12ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   272.0±13.35µs    10.8 MB/sec    1.02   278.1±19.90µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.8±0.27ms     5.3 MB/sec    1.00      4.8±0.24ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
