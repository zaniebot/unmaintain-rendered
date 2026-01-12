```yaml
number: 4351
title: "Use short-import for `HashMap`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/hash
created_at: 2023-05-10T15:36:22Z
updated_at: 2023-05-10T16:20:34Z
url: https://github.com/astral-sh/ruff/pull/4351
synced_at: 2026-01-12T03:56:39Z
```

# Use short-import for `HashMap`

---

_Pull request opened by @charliermarsh on 2023-05-10 15:36_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-10 15:46_

---

_Closed by @charliermarsh on 2023-05-10 15:46_

---

_Branch deleted on 2023-05-10 15:46_

---

_Comment by @github-actions[bot] on 2023-05-10 16:20_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.03ms     2.9 MB/sec    1.00     13.9±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.4±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    417.6±1.93µs     7.1 MB/sec    1.00    418.6±0.80µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1476.6±2.88µs    11.3 MB/sec    1.00   1477.3±4.63µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.1±0.21µs    18.1 MB/sec    1.00    163.7±1.32µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.5±0.00ms     7.4 MB/sec    1.00      5.5±0.00ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1070.2±0.68µs    15.6 MB/sec    1.00   1066.4±2.59µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    108.5±0.13µs    27.2 MB/sec    1.00    108.8±0.15µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    10.9 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.01     25.6±0.95ms  1625.4 KB/sec     1.00     25.5±1.11ms  1635.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      6.3±0.37ms     2.6 MB/sec     1.00      6.3±0.40ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   730.8±50.31µs     4.0 MB/sec     1.00   730.4±53.32µs     4.0 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.8±0.58ms     2.4 MB/sec     1.00     10.7±0.56ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.01     12.8±0.51ms     3.2 MB/sec     1.00     12.7±0.95ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.7±0.18ms     6.2 MB/sec     1.00      2.6±0.12ms     6.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   305.3±30.48µs     9.7 MB/sec     1.00   303.0±25.06µs     9.7 MB/sec
linter/default-rules/pydantic/types.py     1.05      5.9±0.36ms     4.3 MB/sec     1.00      5.6±0.29ms     4.6 MB/sec
parser/large/dataset.py                    1.08     10.6±0.44ms     3.8 MB/sec     1.00      9.8±0.46ms     4.1 MB/sec
parser/numpy/ctypeslib.py                  1.04  1989.0±123.06µs     8.4 MB/sec    1.00  1913.5±121.81µs     8.7 MB/sec
parser/numpy/globals.py                    1.06   201.0±15.73µs    14.7 MB/sec     1.00   189.3±11.99µs    15.6 MB/sec
parser/pydantic/types.py                   1.08      4.5±0.25ms     5.7 MB/sec     1.00      4.2±0.21ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
