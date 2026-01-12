```yaml
number: 4137
title: "Tweak rule documentation for `B008`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/doc
created_at: 2023-04-28T01:24:16Z
updated_at: 2023-04-28T02:02:38Z
url: https://github.com/astral-sh/ruff/pull/4137
synced_at: 2026-01-12T04:28:19Z
```

# Tweak rule documentation for `B008`

---

_Pull request opened by @charliermarsh on 2023-04-28 01:24_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-28 01:29_

---

_Closed by @charliermarsh on 2023-04-28 01:29_

---

_Branch deleted on 2023-04-28 01:29_

---

_Comment by @github-actions[bot] on 2023-04-28 01:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     14.4±0.03ms     2.8 MB/sec    1.00     13.9±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    427.6±0.70µs     6.9 MB/sec    1.00    422.0±0.58µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.0±0.01ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.08      7.5±0.01ms     5.4 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06   1585.1±3.23µs    10.5 MB/sec    1.00   1492.2±4.18µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.04    174.9±0.28µs    16.9 MB/sec    1.00    167.6±0.86µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.3±0.00ms     7.7 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.5±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1060.6±0.50µs    15.7 MB/sec    1.01   1070.1±2.88µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    107.9±0.14µs    27.3 MB/sec    1.00    108.2±0.15µs    27.3 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.01      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     22.3±1.09ms  1865.6 KB/sec     1.02     22.7±1.42ms  1832.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.12      6.4±0.57ms     2.6 MB/sec     1.00      5.7±0.34ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.08   704.4±65.45µs     4.2 MB/sec     1.00   653.4±42.93µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.55ms     2.8 MB/sec     1.01      9.2±0.56ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.89ms     3.7 MB/sec     1.05     11.5±0.49ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.14ms     7.0 MB/sec     1.00      2.4±0.12ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   273.5±19.08µs    10.8 MB/sec     1.04   283.2±24.18µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.26ms     5.2 MB/sec     1.09      5.4±0.35ms     4.7 MB/sec
parser/large/dataset.py                    1.00      9.0±0.55ms     4.5 MB/sec     1.06      9.5±0.48ms     4.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1737.9±104.66µs     9.6 MB/sec    1.07  1852.9±94.35µs     9.0 MB/sec
parser/numpy/globals.py                    1.12   187.5±11.58µs    15.7 MB/sec     1.00   167.8±11.81µs    17.6 MB/sec
parser/pydantic/types.py                   1.00      3.9±0.18ms     6.5 MB/sec     1.00      3.9±0.21ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
