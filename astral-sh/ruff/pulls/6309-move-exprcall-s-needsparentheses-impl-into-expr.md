```yaml
number: 6309
title: "Move `ExprCall`'s `NeedsParentheses` impl into `expr_call.rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/expr-call
created_at: 2023-08-03T15:52:37Z
updated_at: 2023-08-03T16:22:18Z
url: https://github.com/astral-sh/ruff/pull/6309
synced_at: 2026-01-12T15:55:21Z
```

# Move `ExprCall`'s `NeedsParentheses` impl into `expr_call.rs`

---

_@charliermarsh_

Accidental move.

---

_@MichaReiser approved on 2023-08-03 15:56_

---

_Label `internal` added by @charliermarsh on 2023-08-03 15:59_

---

_Merged by @charliermarsh on 2023-08-03 16:01_

---

_Closed by @charliermarsh on 2023-08-03 16:01_

---

_Branch deleted on 2023-08-03 16:01_

---

_Comment by @github-actions[bot] on 2023-08-03 16:22_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.9±0.41ms     3.7 MB/sec    1.00     10.5±0.38ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.08ms     7.8 MB/sec    1.00      2.1±0.07ms     8.1 MB/sec
formatter/numpy/globals.py                 1.02   233.9±12.23µs    12.6 MB/sec    1.00   229.9±10.77µs    12.8 MB/sec
formatter/pydantic/types.py                1.05      4.6±0.19ms     5.6 MB/sec    1.00      4.4±0.16ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     14.9±0.42ms     2.7 MB/sec    1.00     14.7±0.44ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.13ms     4.5 MB/sec    1.00      3.7±0.13ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   522.6±21.25µs     5.6 MB/sec    1.00   522.1±26.61µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.23ms     3.8 MB/sec    1.00      6.7±0.20ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.22ms     5.3 MB/sec    1.01      7.8±0.24ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1600.1±56.29µs    10.4 MB/sec    1.00  1588.9±69.61µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.8±7.32µs    15.5 MB/sec    1.01    191.1±8.88µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.09ms     7.5 MB/sec    1.00      3.4±0.11ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.23ms     4.1 MB/sec    1.02     10.2±0.17ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1935.8±26.77µs     8.6 MB/sec    1.01  1959.2±32.76µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    199.0±4.64µs    14.8 MB/sec    1.03    204.0±9.25µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.09ms     6.0 MB/sec    1.02      4.3±0.07ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.20ms     2.9 MB/sec    1.00     13.9±0.22ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.09ms     4.5 MB/sec    1.00      3.7±0.09ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   374.2±12.30µs     7.9 MB/sec    1.01   378.3±16.98µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.16ms     4.0 MB/sec    1.00      6.3±0.12ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.12ms     5.5 MB/sec    1.00      7.4±0.10ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1478.9±27.40µs    11.3 MB/sec    1.00  1484.8±19.97µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    147.8±3.72µs    20.0 MB/sec    1.00    146.9±2.67µs    20.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.8 MB/sec    1.00      3.3±0.07ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
