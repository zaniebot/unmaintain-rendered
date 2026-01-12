```yaml
number: 3864
title: "Remove `uses_magic_variable_access` dependence on `Checker`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/classify
created_at: 2023-04-03T16:08:52Z
updated_at: 2023-04-03T16:35:28Z
url: https://github.com/astral-sh/ruff/pull/3864
synced_at: 2026-01-12T04:28:19Z
```

# Remove `uses_magic_variable_access` dependence on `Checker`

---

_Pull request opened by @charliermarsh on 2023-04-03 16:08_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-03 16:22_

---

_Closed by @charliermarsh on 2023-04-03 16:22_

---

_Branch deleted on 2023-04-03 16:22_

---

_Comment by @github-actions[bot] on 2023-04-03 16:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.1±0.44ms     2.7 MB/sec    1.00     15.0±0.51ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.19ms     4.4 MB/sec    1.00      3.8±0.16ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   481.8±20.72µs     6.1 MB/sec    1.13   542.9±23.96µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.6±0.34ms     3.9 MB/sec    1.00      6.5±0.24ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.36ms     5.4 MB/sec    1.05      7.9±0.49ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1689.9±76.42µs     9.9 MB/sec    1.01  1715.2±76.52µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   201.4±13.12µs    14.7 MB/sec    1.07   216.4±18.99µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.15ms     7.2 MB/sec    1.09      3.8±0.32ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     18.0±0.55ms     2.3 MB/sec    1.00     17.8±0.47ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.8±0.21ms     3.5 MB/sec    1.00      4.7±0.20ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   576.4±33.39µs     5.1 MB/sec    1.04   599.0±39.06µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.0±0.36ms     3.2 MB/sec    1.00      7.9±0.37ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      9.5±0.39ms     4.3 MB/sec    1.00      9.4±0.54ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.10ms     8.1 MB/sec    1.01      2.1±0.10ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   222.3±14.94µs    13.3 MB/sec    1.04   231.1±19.83µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.24ms     5.8 MB/sec    1.00      4.3±0.19ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
