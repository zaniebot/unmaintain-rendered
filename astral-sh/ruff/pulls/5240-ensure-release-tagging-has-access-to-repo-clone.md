```yaml
number: 5240
title: Ensure release tagging has access to repo clone
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/tag
created_at: 2023-06-21T04:06:08Z
updated_at: 2023-06-21T17:36:55Z
url: https://github.com/astral-sh/ruff/pull/5240
synced_at: 2026-01-12T15:55:18Z
```

# Ensure release tagging has access to repo clone

---

_@charliermarsh_

## Summary

The [release failed](https://github.com/astral-sh/ruff/actions/runs/5329733171/jobs/9656004063), but late enough that I was able to do the remaining steps manually. The issue here is that the tagging step requires that we clone the repo. I split the upload (to PyPI), tagging (in Git), and publishing (to GitHub Releases) phases into their own steps, since they need different resources + permissions anyway.


---

_Review requested from @konstin by @charliermarsh on 2023-06-21 04:06_

---

_Label `release` added by @charliermarsh on 2023-06-21 04:06_

---

_Comment by @github-actions[bot] on 2023-06-21 04:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   1382.0±4.45µs    12.0 MB/sec    1.00   1374.4±4.64µs    12.1 MB/sec
formatter/numpy/globals.py                 1.00    133.4±0.28µs    22.1 MB/sec    1.00    133.4±1.01µs    22.1 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.11ms     3.0 MB/sec    1.02     14.0±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    362.7±0.95µs     8.1 MB/sec    1.00    362.2±2.29µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.05ms     4.2 MB/sec    1.01      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.7 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1470.4±2.56µs    11.3 MB/sec    1.00   1470.5±3.12µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.3±0.26µs    19.0 MB/sec    1.00    155.9±0.20µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.00ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.17      8.8±0.08ms     4.6 MB/sec    1.00      7.5±0.05ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.13  1749.4±30.95µs     9.5 MB/sec    1.00  1545.8±49.22µs    10.8 MB/sec
formatter/numpy/globals.py                 1.09    168.1±3.96µs    17.6 MB/sec    1.00    153.9±5.03µs    19.2 MB/sec
formatter/pydantic/types.py                1.15      3.6±0.04ms     7.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.17ms     2.7 MB/sec    1.00     15.0±0.12ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    485.2±7.44µs     6.1 MB/sec    1.00    481.9±6.26µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.7±0.06ms     3.8 MB/sec    1.00      6.7±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.04ms     5.2 MB/sec    1.00      7.8±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1667.1±14.31µs    10.0 MB/sec    1.00  1660.8±16.17µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    192.9±2.83µs    15.3 MB/sec    1.00    190.9±3.31µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-06-21 05:30_

---

_Merged by @charliermarsh on 2023-06-21 14:24_

---

_Closed by @charliermarsh on 2023-06-21 14:24_

---

_Branch deleted on 2023-06-21 14:24_

---

_@charliermarsh reviewed on 2023-06-21 17:36_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:494 on 2023-06-21 17:36_

Wait, this should be `true`.

---
