```yaml
number: 4441
title: Revert change to re-run release on tag update
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/release
created_at: 2023-05-15T15:30:01Z
updated_at: 2023-05-15T16:04:22Z
url: https://github.com/astral-sh/ruff/pull/4441
synced_at: 2026-01-12T03:50:03Z
```

# Revert change to re-run release on tag update

---

_Pull request opened by @charliermarsh on 2023-05-15 15:30_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-15 15:48_

---

_Closed by @charliermarsh on 2023-05-15 15:48_

---

_Branch deleted on 2023-05-15 15:48_

---

_Comment by @github-actions[bot] on 2023-05-15 15:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.11ms     2.9 MB/sec    1.00     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.6±1.22µs     7.2 MB/sec    1.00    414.5±0.89µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.01      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1439.7±5.50µs    11.6 MB/sec    1.01   1447.3±3.94µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.7±0.25µs    18.6 MB/sec    1.01    160.2±0.65µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.02      5.5±0.01ms     7.4 MB/sec    1.00      5.4±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1056.8±0.75µs    15.8 MB/sec    1.00   1042.9±0.81µs    16.0 MB/sec
parser/numpy/globals.py                    1.01    106.5±0.18µs    27.7 MB/sec    1.00    105.3±0.15µs    28.0 MB/sec
parser/pydantic/types.py                   1.02      2.3±0.01ms    11.0 MB/sec    1.00      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     24.3±0.79ms  1711.0 KB/sec    1.00     23.9±0.86ms  1742.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.8±0.23ms     2.9 MB/sec    1.03      6.0±0.20ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   690.1±49.29µs     4.3 MB/sec    1.00   673.4±27.13µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.39ms     2.6 MB/sec    1.02     10.1±0.45ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.4±0.41ms     3.6 MB/sec    1.07     12.1±0.87ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10      2.4±0.08ms     6.9 MB/sec    1.00      2.2±0.12ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.06   284.2±14.41µs    10.4 MB/sec    1.00   267.6±18.21µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.09      5.2±0.19ms     4.9 MB/sec    1.00      4.8±0.21ms     5.4 MB/sec
parser/large/dataset.py                    1.00      9.0±0.27ms     4.5 MB/sec    1.03      9.3±0.29ms     4.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1786.5±78.64µs     9.3 MB/sec    1.02  1825.0±61.14µs     9.1 MB/sec
parser/numpy/globals.py                    1.00    185.3±9.84µs    15.9 MB/sec    1.00    184.5±9.61µs    16.0 MB/sec
parser/pydantic/types.py                   1.00      4.0±0.11ms     6.4 MB/sec    1.02      4.0±0.15ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
