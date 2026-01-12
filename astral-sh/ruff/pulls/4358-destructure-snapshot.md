```yaml
number: 4358
title: "Destructure `Snapshot`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/snapshot
created_at: 2023-05-10T19:32:27Z
updated_at: 2023-05-10T20:11:59Z
url: https://github.com/astral-sh/ruff/pull/4358
synced_at: 2026-01-12T15:55:15Z
```

# Destructure `Snapshot`

---

_@charliermarsh_

_No description provided._

---

_Renamed from "Remove `Copy` and destructure `Snapshot`" to "Destructure `Snapshot`" by @charliermarsh on 2023-05-10 19:41_

---

_Merged by @charliermarsh on 2023-05-10 19:46_

---

_Closed by @charliermarsh on 2023-05-10 19:46_

---

_Branch deleted on 2023-05-10 19:46_

---

_Comment by @github-actions[bot] on 2023-05-10 19:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.04     14.3±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.02      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.2±0.74µs     7.2 MB/sec    1.01    417.6±0.83µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.03      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.09      7.4±0.04ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1455.5±1.96µs    11.4 MB/sec    1.06   1541.7±1.53µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.8±0.28µs    18.4 MB/sec    1.04    167.6±0.29µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.06      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.01      5.4±0.00ms     7.6 MB/sec    1.00      5.3±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1045.0±0.57µs    15.9 MB/sec    1.00   1046.0±0.81µs    15.9 MB/sec
parser/numpy/globals.py                    1.00    105.9±0.25µs    27.9 MB/sec    1.00    105.7±0.15µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.00      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.22ms     2.5 MB/sec    1.00     16.2±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.1±6.86µs     6.0 MB/sec    1.01    496.1±7.18µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.8±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     5.0 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.2±24.20µs     9.6 MB/sec    1.00  1731.4±15.89µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.6±3.75µs    15.4 MB/sec    1.02    195.9±5.83µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     6.9 MB/sec    1.00      3.7±0.06ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.02      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1266.3±11.58µs    13.1 MB/sec    1.02  1292.1±19.72µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    129.8±2.03µs    22.7 MB/sec    1.02    132.0±5.65µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.02      2.9±0.04ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
