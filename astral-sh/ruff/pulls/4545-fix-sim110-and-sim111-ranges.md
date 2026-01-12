```yaml
number: 4545
title: Fix SIM110 and SIM111 ranges
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-sim110-sim111-ranges
created_at: 2023-05-20T16:33:53Z
updated_at: 2023-05-20T17:08:59Z
url: https://github.com/astral-sh/ruff/pull/4545
synced_at: 2026-01-12T03:50:03Z
```

# Fix SIM110 and SIM111 ranges

---

_Pull request opened by @JonathanPlasse on 2023-05-20 16:33_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-20 16:40_

---

_Closed by @charliermarsh on 2023-05-20 16:40_

---

_Branch deleted on 2023-05-20 16:44_

---

_Comment by @github-actions[bot] on 2023-05-20 16:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.8±0.15ms     2.7 MB/sec    1.00     14.5±0.14ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.02ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    432.2±0.67µs     6.8 MB/sec    1.00    422.9±0.78µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.07ms     4.3 MB/sec    1.00      5.9±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.03ms     5.9 MB/sec    1.00      6.8±0.06ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1477.7±4.62µs    11.3 MB/sec    1.00   1463.1±5.40µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    164.6±0.25µs    17.9 MB/sec    1.00    162.0±0.87µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1062.9±0.73µs    15.7 MB/sec    1.00   1061.4±0.85µs    15.7 MB/sec
parser/numpy/globals.py                    1.01    109.9±0.40µs    26.9 MB/sec    1.00    108.9±0.18µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.59ms     2.3 MB/sec    1.00     17.4±0.41ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.10ms     3.9 MB/sec    1.01      4.4±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   503.2±11.46µs     5.9 MB/sec    1.00   501.7±11.57µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.16ms     3.6 MB/sec    1.02      7.3±0.21ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.12ms     4.8 MB/sec    1.03      8.7±0.19ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1751.8±22.59µs     9.5 MB/sec    1.03  1797.8±38.53µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.1±3.53µs    15.0 MB/sec    1.02    200.4±6.24µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.09ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
parser/large/dataset.py                    1.10      7.4±0.09ms     5.5 MB/sec    1.00      6.7±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.07  1380.0±25.54µs    12.1 MB/sec    1.00  1291.7±16.60µs    12.9 MB/sec
parser/numpy/globals.py                    1.06    140.8±2.81µs    21.0 MB/sec    1.00    133.1±2.77µs    22.2 MB/sec
parser/pydantic/types.py                   1.06      3.1±0.04ms     8.3 MB/sec    1.00      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
