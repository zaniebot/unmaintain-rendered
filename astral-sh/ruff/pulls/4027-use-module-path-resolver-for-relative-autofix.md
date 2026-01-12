```yaml
number: 4027
title: Use module path resolver for relative autofix
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fix-relative
created_at: 2023-04-19T18:12:16Z
updated_at: 2023-04-19T19:05:32Z
url: https://github.com/astral-sh/ruff/pull/4027
synced_at: 2026-01-12T15:55:14Z
```

# Use module path resolver for relative autofix

---

_@charliermarsh_

We have a new method that takes a relative import level, module name, and caller path, to produce the fully-qualified, absolute path for a given import. So we might as well use it in the relative import autofixer, which today re-implements all of that logic on its own.

---

_Comment by @github-actions[bot] on 2023-04-19 18:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.05ms     2.7 MB/sec    1.00     15.2±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.3 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.4±3.42µs     7.1 MB/sec    1.01    416.9±1.70µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.02ms     3.9 MB/sec    1.00      6.5±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.9±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1737.4±7.86µs     9.6 MB/sec    1.00  1743.7±14.25µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.8±0.34µs    16.2 MB/sec    1.00    182.5±0.70µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.2±0.01ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1236.7±1.03µs    13.5 MB/sec    1.00   1240.4±1.19µs    13.4 MB/sec
parser/numpy/globals.py                    1.01    124.8±0.22µs    23.6 MB/sec    1.00    124.1±0.30µs    23.8 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.4 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.24ms     2.5 MB/sec    1.00     16.4±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.4±9.57µs     5.9 MB/sec    1.00   503.8±13.05µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.12ms     3.6 MB/sec    1.00      6.9±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.10ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1831.3±38.26µs     9.1 MB/sec    1.01  1842.8±43.57µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.0±7.62µs    14.8 MB/sec    1.01    200.5±5.89µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.01      3.8±0.11ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.6±0.10ms     6.1 MB/sec    1.00      6.5±0.07ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1245.2±23.43µs    13.4 MB/sec    1.00  1247.3±21.73µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    124.5±2.94µs    23.7 MB/sec    1.00    125.0±3.27µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.07ms     9.1 MB/sec    1.02      2.9±0.08ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-19 18:43_

---

_Closed by @charliermarsh on 2023-04-19 18:43_

---

_Branch deleted on 2023-04-19 18:43_

---
