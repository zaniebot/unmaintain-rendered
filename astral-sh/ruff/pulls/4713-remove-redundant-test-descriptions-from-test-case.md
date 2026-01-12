```yaml
number: 4713
title: "Remove redundant test descriptions from `#test_case` macros"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/test-description
created_at: 2023-05-29T21:50:23Z
updated_at: 2023-05-29T22:31:12Z
url: https://github.com/astral-sh/ruff/pull/4713
synced_at: 2026-01-12T03:50:03Z
```

# Remove redundant test descriptions from `#test_case` macros

---

_Pull request opened by @charliermarsh on 2023-05-29 21:50_

## Summary

These may have been necessary at some point in the past, but today, they have no effect on our snapshots etc., and they just end up containing duplicative information from the file and rule name.

## Test Plan

- Ran `cargo test`; verified that it passed without error.
- Invalidated a snapshot, and ran `cargo test`; verified that it failed.


---

_Renamed from "Remove redundant test descriptions from #test_case macros" to "Remove redundant test descriptions from `#test_case` macros" by @charliermarsh on 2023-05-29 21:50_

---

_Comment by @github-actions[bot] on 2023-05-29 22:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.04ms     2.8 MB/sec    1.01     14.5±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.8 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.2±0.71µs     6.9 MB/sec    1.01    431.7±3.98µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      6.0±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1509.9±1.74µs    11.0 MB/sec    1.01   1517.8±3.58µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.9±0.19µs    17.2 MB/sec    1.01    172.9±4.33µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.1±0.01ms     7.9 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1020.4±0.56µs    16.3 MB/sec    1.00   1017.7±1.14µs    16.4 MB/sec
parser/numpy/globals.py                    1.00    105.3±0.15µs    28.0 MB/sec    1.01    105.8±0.65µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.5 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.2±0.05ms     3.3 MB/sec    1.00     12.1±0.14ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.02ms     5.3 MB/sec    1.01      3.2±0.03ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    326.4±5.07µs     9.0 MB/sec    1.01    329.6±7.77µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.2±0.04ms     4.9 MB/sec    1.01      5.3±0.06ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      6.2±0.04ms     6.6 MB/sec    1.00      6.1±0.05ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1307.8±27.22µs    12.7 MB/sec    1.00  1301.7±10.92µs    12.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.0±1.40µs    20.9 MB/sec    1.00    141.4±1.42µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.8±0.02ms     9.1 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
parser/large/dataset.py                    1.01      4.8±0.02ms     8.4 MB/sec    1.00      4.8±0.02ms     8.5 MB/sec
parser/numpy/ctypeslib.py                  1.01    910.8±7.05µs    18.3 MB/sec    1.00    905.6±6.24µs    18.4 MB/sec
parser/numpy/globals.py                    1.00     94.1±0.83µs    31.3 MB/sec    1.00     93.9±0.75µs    31.4 MB/sec
parser/pydantic/types.py                   1.01      2.1±0.02ms    12.4 MB/sec    1.00      2.0±0.02ms    12.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-29 22:23_

---

_Closed by @charliermarsh on 2023-05-29 22:23_

---

_Branch deleted on 2023-05-29 22:23_

---
