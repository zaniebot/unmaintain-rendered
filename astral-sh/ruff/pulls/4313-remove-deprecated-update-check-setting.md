```yaml
number: 4313
title: "Remove deprecated `update-check` setting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
assignees: []
merged: true
base: main
head: charlie/update-check
created_at: 2023-05-09T16:28:38Z
updated_at: 2023-05-09T17:35:40Z
url: https://github.com/astral-sh/ruff/pull/4313
synced_at: 2026-01-12T15:55:15Z
```

# Remove deprecated `update-check` setting

---

_@charliermarsh_

Breaking in that users with `update-check = false` in their configuration file should now remove it.

---

_Label `breaking` added by @charliermarsh on 2023-05-09 16:28_

---

_Renamed from "Remove deprecated update-check setting" to "Remove deprecated `update-check` setting" by @charliermarsh on 2023-05-09 16:28_

---

_Merged by @charliermarsh on 2023-05-09 17:10_

---

_Closed by @charliermarsh on 2023-05-09 17:10_

---

_Branch deleted on 2023-05-09 17:10_

---

_Comment by @github-actions[bot] on 2023-05-09 17:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.1±0.02ms     2.9 MB/sec    1.00     14.0±0.01ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.1±1.26µs     7.1 MB/sec    1.00    413.1±2.37µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      7.0±0.01ms     5.8 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1495.3±3.41µs    11.1 MB/sec    1.00   1477.4±4.79µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.4±0.93µs    18.1 MB/sec    1.00    163.3±2.97µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.02ms     7.5 MB/sec    1.01      5.5±0.00ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1058.8±0.91µs    15.7 MB/sec    1.01   1067.9±0.85µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    107.6±0.17µs    27.4 MB/sec    1.00    107.9±0.31µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.18ms     2.5 MB/sec    1.01     16.6±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.09      4.6±0.95ms     3.6 MB/sec    1.00      4.2±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   430.2±16.07µs     6.9 MB/sec    1.01    435.0±8.58µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.54ms     3.5 MB/sec    1.00      7.1±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.03ms     4.9 MB/sec    1.01      8.4±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1742.6±13.16µs     9.6 MB/sec    1.01  1762.2±25.90µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.4±1.63µs    15.8 MB/sec    1.01    188.8±4.77µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.8 MB/sec    1.01      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      7.0±0.02ms     5.8 MB/sec    1.00      7.0±0.03ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1324.3±8.97µs    12.6 MB/sec    1.00  1329.4±14.58µs    12.5 MB/sec
parser/numpy/globals.py                    1.00    136.4±1.86µs    21.6 MB/sec    1.00    136.1±0.82µs    21.7 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.02ms     8.6 MB/sec    1.00      3.0±0.02ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
