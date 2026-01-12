```yaml
number: 4234
title: "Use `NodeId` for `Binding` source"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/binding-id
created_at: 2023-05-05T01:08:13Z
updated_at: 2023-05-06T16:44:42Z
url: https://github.com/astral-sh/ruff/pull/4234
synced_at: 2026-01-12T03:56:39Z
```

# Use `NodeId` for `Binding` source

---

_Pull request opened by @charliermarsh on 2023-05-05 01:08_

## Summary

Rather than storing the source as a `&Stmt`, we can instead store the `NodeId` for the corresponding `Stmt` and look it up as needed.

This does require lookups in a few additional spots, but it also simplifies some of the logic, e.g., in `branch_detection.rs`.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-05 01:08_

---

_Comment by @github-actions[bot] on 2023-05-05 01:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.07     15.9±0.14ms     2.6 MB/sec    1.00     14.9±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.7±0.01ms     4.5 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.03    372.2±1.07µs     7.9 MB/sec    1.00    361.7±0.64µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.5±0.02ms     3.9 MB/sec    1.00      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.14      8.5±0.01ms     4.8 MB/sec    1.00      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10   1716.9±2.74µs     9.7 MB/sec    1.00   1555.4±4.86µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.07    179.0±0.27µs    16.5 MB/sec    1.00    167.9±3.09µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.7±0.01ms     6.9 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      6.0±0.00ms     6.8 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1156.6±0.85µs    14.4 MB/sec    1.00   1155.8±2.89µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    117.9±0.23µs    25.0 MB/sec    1.00    117.7±1.16µs    25.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.00      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.19ms     2.5 MB/sec    1.01     16.5±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.13ms     4.0 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.6±7.06µs     6.1 MB/sec    1.01    493.5±9.66µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.19ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.06ms     4.9 MB/sec    1.02      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1759.5±48.31µs     9.5 MB/sec    1.01  1780.6±16.58µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.5±6.44µs    14.8 MB/sec    1.02    202.5±9.41µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.01      3.8±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.03      6.8±0.18ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1272.5±15.34µs    13.1 MB/sec    1.01  1291.4±15.86µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.8±2.68µs    22.4 MB/sec    1.01    133.1±2.19µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.01      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-05 06:52_

---

_Merged by @charliermarsh on 2023-05-06 16:20_

---

_Closed by @charliermarsh on 2023-05-06 16:20_

---

_Branch deleted on 2023-05-06 16:20_

---
