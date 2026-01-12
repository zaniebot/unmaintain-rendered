```yaml
number: 3843
title: "Move logging resolver into `logging.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/useless-iv
created_at: 2023-04-01T03:31:09Z
updated_at: 2023-04-01T04:11:59Z
url: https://github.com/astral-sh/ruff/pull/3843
synced_at: 2026-01-12T04:28:19Z
```

# Move logging resolver into `logging.rs`

---

_Pull request opened by @charliermarsh on 2023-04-01 03:31_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-01 03:50_

---

_Closed by @charliermarsh on 2023-04-01 03:50_

---

_Branch deleted on 2023-04-01 03:50_

---

_Comment by @github-actions[bot] on 2023-04-01 04:11_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.0±0.58ms     2.3 MB/sec    1.02     18.4±0.61ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.02      4.5±0.15ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   591.2±22.83µs     5.0 MB/sec    1.00   579.4±16.32µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.24ms     3.3 MB/sec    1.01      7.8±0.35ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.21ms     4.5 MB/sec    1.02      9.3±0.23ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.06ms     8.2 MB/sec    1.01      2.1±0.07ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    244.7±7.61µs    12.1 MB/sec    1.02   248.8±10.63µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.16ms     5.9 MB/sec    1.01      4.3±0.17ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.24ms     2.6 MB/sec    1.01     16.2±0.34ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.02      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.7±7.57µs     6.0 MB/sec    1.00    496.9±8.35µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.11ms     3.8 MB/sec    1.04      7.0±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.10ms     4.9 MB/sec    1.01      8.4±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1803.4±35.44µs     9.2 MB/sec    1.03  1864.1±31.30µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.9±3.81µs    15.3 MB/sec    1.02    195.9±4.99µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.7 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
