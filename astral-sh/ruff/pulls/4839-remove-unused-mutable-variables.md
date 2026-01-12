```yaml
number: 4839
title: Remove unused mutable variables
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: internal/unused-mut
created_at: 2023-06-03T21:17:51Z
updated_at: 2023-06-03T21:46:51Z
url: https://github.com/astral-sh/ruff/pull/4839
synced_at: 2026-01-12T03:50:04Z
```

# Remove unused mutable variables

---

_Pull request opened by @zanieb on 2023-06-03 21:17_

The nightly rust compiler warns on these `mut` variables that are not mutated.

Fixed with `cargo +nightly fix --lib -p ruff`.


---

_Comment by @github-actions[bot] on 2023-06-03 21:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.9±0.46ms     2.3 MB/sec    1.06     19.1±0.61ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.14ms     3.8 MB/sec    1.04      4.5±0.15ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.08   584.4±28.62µs     5.0 MB/sec    1.00   539.3±16.81µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.0±0.45ms     3.2 MB/sec    1.00      7.7±0.26ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.02      8.8±0.23ms     4.6 MB/sec    1.00      8.6±0.23ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1941.4±64.83µs     8.6 MB/sec    1.00  1932.3±65.45µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.07   253.9±26.73µs    11.6 MB/sec    1.00   237.1±11.55µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.3±0.18ms     5.9 MB/sec    1.00      4.2±0.26ms     6.1 MB/sec
parser/large/dataset.py                    1.00      7.0±0.37ms     5.8 MB/sec    1.03      7.2±0.26ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1367.7±55.39µs    12.2 MB/sec    1.03  1412.6±36.94µs    11.8 MB/sec
parser/numpy/globals.py                    1.00    136.1±7.96µs    21.7 MB/sec    1.06    143.7±6.28µs    20.5 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.16ms     8.4 MB/sec    1.02      3.1±0.11ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.2±0.07ms     3.3 MB/sec    1.00     12.2±0.08ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.02ms     5.3 MB/sec    1.00      3.2±0.03ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    323.5±5.84µs     9.1 MB/sec    1.00    322.6±5.25µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.2±0.03ms     4.9 MB/sec    1.00      5.2±0.03ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.01      6.2±0.02ms     6.6 MB/sec    1.00      6.2±0.04ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1301.8±8.98µs    12.8 MB/sec    1.00  1297.9±10.28µs    12.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.0±1.11µs    20.9 MB/sec    1.01    142.3±1.56µs    20.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.02ms     9.1 MB/sec    1.01      2.8±0.03ms     9.0 MB/sec
parser/large/dataset.py                    1.12      5.4±0.02ms     7.5 MB/sec    1.00      4.8±0.03ms     8.4 MB/sec
parser/numpy/ctypeslib.py                  1.09    996.3±5.44µs    16.7 MB/sec    1.00   912.4±13.89µs    18.2 MB/sec
parser/numpy/globals.py                    1.08    101.3±0.96µs    29.1 MB/sec    1.00     93.9±1.08µs    31.4 MB/sec
parser/pydantic/types.py                   1.10      2.3±0.06ms    11.3 MB/sec    1.00      2.1±0.02ms    12.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-06-03 21:30_

---

_@charliermarsh approved on 2023-06-03 21:31_

Nice. It's interesting that removing the `mut` "works" (i.e., compiles) on 1.69.0, but isn't flagged as unnecessary by that version of the compiler.

---

_Merged by @charliermarsh on 2023-06-03 21:31_

---

_Closed by @charliermarsh on 2023-06-03 21:31_

---
