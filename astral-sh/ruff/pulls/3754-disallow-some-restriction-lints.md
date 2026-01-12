```yaml
number: 3754
title: Disallow some restriction lints
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/clippy
created_at: 2023-03-26T23:08:57Z
updated_at: 2023-03-26T23:32:43Z
url: https://github.com/astral-sh/ruff/pull/3754
synced_at: 2026-01-12T04:39:45Z
```

# Disallow some restriction lints

---

_Pull request opened by @charliermarsh on 2023-03-26 23:08_

_No description provided._

---

_Merged by @charliermarsh on 2023-03-26 23:20_

---

_Closed by @charliermarsh on 2023-03-26 23:20_

---

_Branch deleted on 2023-03-26 23:20_

---

_Comment by @github-actions[bot] on 2023-03-26 23:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.04ms     2.9 MB/sec    1.00     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.4±1.40µs     6.8 MB/sec    1.00    431.6±3.81µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.04ms     5.2 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1698.3±11.49µs     9.8 MB/sec    1.00   1690.2±5.12µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.6±0.30µs    16.8 MB/sec    1.00    176.2±1.05µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.00ms     7.0 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.4±0.90ms     2.1 MB/sec    1.00     19.3±0.84ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.2±0.28ms     3.2 MB/sec    1.00      5.1±0.22ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   653.5±38.98µs     4.5 MB/sec    1.02   669.4±48.24µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.46ms     3.0 MB/sec    1.01      8.6±0.51ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.44ms     4.0 MB/sec    1.01     10.4±0.51ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.4 MB/sec    1.01      2.3±0.10ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.2±18.04µs    11.7 MB/sec    1.02   258.1±15.80µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.23ms     5.3 MB/sec    1.01      4.9±0.27ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
