```yaml
number: 4911
title: "Add `contents: write` permission to release step"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/release
created_at: 2023-06-07T01:03:59Z
updated_at: 2023-06-07T12:42:28Z
url: https://github.com/astral-sh/ruff/pull/4911
synced_at: 2026-01-12T15:55:17Z
```

# Add `contents: write` permission to release step

---

_@charliermarsh_

## Summary

In the most recent release, the "add the binaries to the GitHub release" step [failed](https://github.com/charliermarsh/ruff/actions/runs/5194110770/jobs/9365856414), due to a missing permission. I think that when we introduced `id-token: write`, it effectively turned off all other permissions (see [docs](https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs)). The `action-gh-release` docs state that they need [`contents: write`](https://github.com/softprops/action-gh-release#permissions), so this PR adds it.


---

_Review requested from @konstin by @charliermarsh on 2023-06-07 01:04_

---

_Renamed from "Add contents: write permission to release step" to "Add `contents: write` permission to release step" by @charliermarsh on 2023-06-07 01:04_

---

_Comment by @github-actions[bot] on 2023-06-07 01:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.6±0.01ms     7.2 MB/sec    1.01      5.7±0.02ms     7.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1184.5±1.02µs    14.1 MB/sec    1.01   1201.7±8.27µs    13.9 MB/sec
formatter/numpy/globals.py                 1.00    138.7±0.23µs    21.3 MB/sec    1.01    139.4±0.78µs    21.2 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.01ms    10.1 MB/sec    1.01      2.5±0.01ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.03ms     2.9 MB/sec    1.03     14.3±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.02      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.7±0.66µs     7.2 MB/sec    1.01    414.3±2.30µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.02      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.06      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1442.1±2.27µs    11.5 MB/sec    1.04   1498.9±3.56µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.4±0.19µs    18.6 MB/sec    1.03    163.1±2.63µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.04      3.1±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.09ms     5.8 MB/sec    1.00      7.0±0.10ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1408.5±22.65µs    11.8 MB/sec    1.02  1443.0±88.61µs    11.5 MB/sec
formatter/numpy/globals.py                 1.00    160.0±3.29µs    18.4 MB/sec    1.00    160.7±4.62µs    18.4 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.05ms     8.3 MB/sec    1.01      3.1±0.07ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.16ms     2.5 MB/sec    1.05     17.3±0.38ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.02      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.8±8.31µs     6.0 MB/sec    1.00   492.6±12.18µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.16ms     3.6 MB/sec    1.00      7.1±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.00      8.2±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.9±21.50µs     9.6 MB/sec    1.01  1737.7±32.59µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.1±7.12µs    15.0 MB/sec    1.00    197.4±7.71µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-07 12:41_

---

_Merged by @charliermarsh on 2023-06-07 12:42_

---

_Closed by @charliermarsh on 2023-06-07 12:42_

---

_Branch deleted on 2023-06-07 12:42_

---
