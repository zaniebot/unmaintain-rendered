```yaml
number: 4796
title: "Remove some matches on `Stmt`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/is_stmt
created_at: 2023-06-02T04:24:50Z
updated_at: 2023-06-02T04:57:29Z
url: https://github.com/astral-sh/ruff/pull/4796
synced_at: 2026-01-12T03:50:03Z
```

# Remove some matches on `Stmt`

---

_Pull request opened by @charliermarsh on 2023-06-02 04:24_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-02 04:24_

---

_Merged by @charliermarsh on 2023-06-02 04:36_

---

_Closed by @charliermarsh on 2023-06-02 04:36_

---

_Branch deleted on 2023-06-02 04:36_

---

_Comment by @github-actions[bot] on 2023-06-02 04:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.03ms     2.9 MB/sec    1.00     14.0±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.5±0.80µs     7.0 MB/sec    1.00    425.5±0.36µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.01ms     5.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1512.4±2.39µs    11.0 MB/sec    1.00   1497.2±3.60µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.5±1.55µs    17.3 MB/sec    1.00    169.7±0.25µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.03      5.3±0.00ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1011.9±1.10µs    16.5 MB/sec    1.02   1032.5±9.80µs    16.1 MB/sec
parser/numpy/globals.py                    1.00    105.2±1.94µs    28.0 MB/sec    1.02    107.0±1.21µs    27.6 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.4 MB/sec    1.01      2.3±0.02ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.06     20.4±1.16ms  2042.6 KB/sec     1.00     19.3±0.97ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.9±0.25ms     3.4 MB/sec     1.00      4.8±0.24ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   568.5±38.30µs     5.2 MB/sec     1.09   622.2±51.77µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.47ms     3.2 MB/sec     1.08      8.5±0.41ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.56ms     4.2 MB/sec     1.06     10.3±0.55ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.14ms     7.7 MB/sec     1.07      2.3±0.24ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   257.5±19.26µs    11.5 MB/sec     1.03   265.4±31.43µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.27ms     5.4 MB/sec     1.02      4.8±0.40ms     5.3 MB/sec
parser/large/dataset.py                    1.00      7.9±0.49ms     5.2 MB/sec     1.05      8.2±0.39ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1486.8±122.05µs    11.2 MB/sec    1.08  1609.2±67.41µs    10.3 MB/sec
parser/numpy/globals.py                    1.00   149.2±10.53µs    19.8 MB/sec     1.05    156.2±9.10µs    18.9 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.19ms     7.6 MB/sec     1.07      3.6±0.16ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
