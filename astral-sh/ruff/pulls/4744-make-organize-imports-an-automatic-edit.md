```yaml
number: 4744
title: Make organize imports an automatic edit
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/auto
created_at: 2023-05-31T04:10:31Z
updated_at: 2023-05-31T05:01:06Z
url: https://github.com/astral-sh/ruff/pull/4744
synced_at: 2026-01-12T03:50:03Z
```

# Make organize imports an automatic edit

---

_Pull request opened by @charliermarsh on 2023-05-31 04:10_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-31 04:29_

---

_Closed by @charliermarsh on 2023-05-31 04:29_

---

_Branch deleted on 2023-05-31 04:29_

---

_Comment by @github-actions[bot] on 2023-05-31 04:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.08     21.8±0.89ms  1914.2 KB/sec     1.00     20.2±0.59ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      5.0±0.31ms     3.3 MB/sec     1.00      4.7±0.16ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   594.9±39.81µs     5.0 MB/sec     1.00   593.0±28.79µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.09      9.0±0.55ms     2.8 MB/sec     1.00      8.3±0.31ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.09     10.3±0.56ms     3.9 MB/sec     1.00      9.5±0.48ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.1±0.12ms     7.9 MB/sec     1.00      2.0±0.08ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.04   241.8±18.30µs    12.2 MB/sec     1.00   233.5±10.85µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.6±0.31ms     5.6 MB/sec     1.00      4.3±0.16ms     6.0 MB/sec
parser/large/dataset.py                    1.04      7.8±0.36ms     5.2 MB/sec     1.00      7.5±0.24ms     5.4 MB/sec
parser/numpy/ctypeslib.py                  1.03  1491.8±114.54µs    11.2 MB/sec    1.00  1442.7±34.17µs    11.5 MB/sec
parser/numpy/globals.py                    1.01   144.2±11.41µs    20.5 MB/sec     1.00    142.7±4.15µs    20.7 MB/sec
parser/pydantic/types.py                   1.04      3.3±0.18ms     7.8 MB/sec     1.00      3.2±0.06ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.15ms     2.5 MB/sec    1.01     16.7±0.22ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.9±7.07µs     5.9 MB/sec    1.00    501.0±4.96µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.7 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     5.0 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1748.1±29.68µs     9.5 MB/sec    1.00  1752.3±19.66µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.8±2.65µs    14.6 MB/sec    1.01    204.8±7.40µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1216.3±13.71µs    13.7 MB/sec    1.01  1223.0±14.94µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    124.6±2.19µs    23.7 MB/sec    1.01    125.4±2.45µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.3 MB/sec    1.01      2.8±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
