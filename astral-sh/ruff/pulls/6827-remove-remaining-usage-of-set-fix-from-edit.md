```yaml
number: 6827
title: "Remove remaining usage of `set_fix_from_edit`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fix
created_at: 2023-08-23T22:15:03Z
updated_at: 2023-08-23T22:55:06Z
url: https://github.com/astral-sh/ruff/pull/6827
synced_at: 2026-01-12T02:45:38Z
```

# Remove remaining usage of `set_fix_from_edit`

---

_Pull request opened by @charliermarsh on 2023-08-23 22:15_

This method is deprecated; we have one last usage, so removing it.

---

_Label `internal` added by @charliermarsh on 2023-08-23 22:15_

---

_Merged by @charliermarsh on 2023-08-23 22:25_

---

_Closed by @charliermarsh on 2023-08-23 22:25_

---

_Branch deleted on 2023-08-23 22:25_

---

_Comment by @github-actions[bot] on 2023-08-23 22:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.04ms    11.3 MB/sec    1.00      3.6±0.03ms    11.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    733.4±4.85µs    22.7 MB/sec    1.00    735.3±2.51µs    22.6 MB/sec
formatter/numpy/globals.py                 1.00     74.8±0.31µs    39.4 MB/sec    1.00     74.9±0.37µs    39.4 MB/sec
formatter/pydantic/types.py                1.00   1470.3±4.43µs    17.3 MB/sec    1.00   1470.1±3.94µs    17.3 MB/sec
linter/all-rules/large/dataset.py          1.01     10.4±0.06ms     3.9 MB/sec    1.00     10.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.01      2.8±0.00ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.7±0.80µs     7.7 MB/sec    1.01    389.4±0.82µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.04ms     4.8 MB/sec    1.01      5.4±0.05ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.02ms     7.5 MB/sec    1.01      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1186.1±5.81µs    14.0 MB/sec    1.02   1206.5±2.34µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.0±0.26µs    21.2 MB/sec    1.01    140.2±1.34µs    21.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.00      2.5±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      5.0±0.18ms     8.1 MB/sec     1.00      5.0±0.20ms     8.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1005.6±47.10µs    16.6 MB/sec     1.00  1001.3±65.24µs    16.6 MB/sec
formatter/numpy/globals.py                 1.00    100.0±5.87µs    29.5 MB/sec     1.00    100.2±6.06µs    29.5 MB/sec
formatter/pydantic/types.py                1.03      2.1±0.09ms    12.2 MB/sec     1.00      2.0±0.10ms    12.6 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.61ms     2.4 MB/sec     1.00     16.8±0.48ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.18ms     3.7 MB/sec     1.02      4.6±0.24ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   576.5±40.69µs     5.1 MB/sec     1.01   583.5±29.93µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.37ms     2.9 MB/sec     1.00      8.6±0.32ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.31ms     4.5 MB/sec     1.01      9.2±0.31ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1951.2±118.96µs     8.5 MB/sec    1.01  1965.4±77.65µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.3±10.72µs    12.3 MB/sec     1.02   245.3±11.14µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.21ms     6.2 MB/sec     1.01      4.2±0.18ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
