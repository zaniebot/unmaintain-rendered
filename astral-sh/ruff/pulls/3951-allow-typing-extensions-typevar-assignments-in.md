```yaml
number: 3951
title: "Allow `typing_extensions.TypeVar` assignments in `.pyi` files"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2023-04-12T20:39:05Z
updated_at: 2023-04-12T21:30:17Z
url: https://github.com/astral-sh/ruff/pull/3951
synced_at: 2026-01-12T15:55:14Z
```

# Allow `typing_extensions.TypeVar` assignments in `.pyi` files

---

_@charliermarsh_

We're still not quite in full compliance with flake8-pyi (the excellent comment [here](https://github.com/charliermarsh/ruff/pull/3861#issuecomment-1494889518) from Alex Waygood outlines flake8-pyi's strategy), but this is definitely an improvement and an oversight as-is.

Closes #3950.

---

_Comment by @github-actions[bot] on 2023-04-12 20:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.03ms     2.9 MB/sec    1.01     14.3±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    465.4±0.63µs     6.3 MB/sec    1.00    465.2±1.03µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.6 MB/sec    1.01      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1647.5±1.52µs    10.1 MB/sec    1.00   1640.2±2.29µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    183.8±0.54µs    16.1 MB/sec    1.00    182.7±2.37µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.25ms     2.4 MB/sec    1.02     17.3±0.43ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.10ms     3.8 MB/sec    1.00      4.2±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   516.7±17.04µs     5.7 MB/sec    1.00   504.6±15.14µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.19ms     3.6 MB/sec    1.00      7.2±0.25ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.5±0.11ms     4.8 MB/sec    1.00      8.3±0.18ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1852.7±44.42µs     9.0 MB/sec    1.00  1791.9±29.97µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.05   204.0±14.89µs    14.5 MB/sec    1.00    193.8±5.58µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.9±0.10ms     6.6 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-12 21:30_

---

_Closed by @charliermarsh on 2023-04-12 21:30_

---

_Branch deleted on 2023-04-12 21:30_

---
