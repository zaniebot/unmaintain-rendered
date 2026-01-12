```yaml
number: 3567
title: "ci(check_ecosystem): add cibuildwheel"
type: pull_request
state: merged
author: henryiii
labels: []
assignees: []
merged: true
base: main
head: henryiii/ci/cibw
created_at: 2023-03-17T01:26:24Z
updated_at: 2023-03-17T02:34:57Z
url: https://github.com/astral-sh/ruff/pull/3567
synced_at: 2026-01-12T04:39:45Z
```

# ci(check_ecosystem): add cibuildwheel

---

_Pull request opened by @henryiii on 2023-03-17 01:26_

Adding cibuildwheel to the ecosystem check. Referenced in https://github.com/charliermarsh/ruff/pull/3390#issuecomment-1472791231


---

_Comment by @github-actions[bot] on 2023-03-17 01:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00     10.4±0.45ms     3.9 MB/sec    1.02     10.6±0.46ms     3.8 MB/sec
linter/numpy/ctypeslib.py    1.01      2.6±0.12ms   128.2 MB/sec    1.00      2.6±0.09ms   129.3 MB/sec
linter/numpy/globals.py      1.00  1363.9±75.64µs   130.7 MB/sec    1.01  1371.2±166.47µs   130.0 MB/sec
linter/pydantic/types.py     1.00      4.8±0.21ms     5.3 MB/sec    1.04      5.0±0.25ms     5.1 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.6±0.07ms     4.7 MB/sec    1.02      8.7±0.08ms     4.7 MB/sec
linter/numpy/ctypeslib.py    1.00      2.8±0.02ms   121.3 MB/sec    1.01      2.8±0.04ms   120.6 MB/sec
linter/numpy/globals.py      1.00  1446.3±20.33µs   123.2 MB/sec    1.01  1466.6±45.29µs   121.5 MB/sec
linter/pydantic/types.py     1.00      4.0±0.05ms     6.4 MB/sec    1.02      4.1±0.08ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-17 02:34_

---

_Closed by @charliermarsh on 2023-03-17 02:34_

---
