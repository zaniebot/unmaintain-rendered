```yaml
number: 4550
title: Fix PLE01310 typo
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-ple01310-type
created_at: 2023-05-20T19:27:45Z
updated_at: 2023-05-20T21:46:07Z
url: https://github.com/astral-sh/ruff/pull/4550
synced_at: 2026-01-12T15:55:15Z
```

# Fix PLE01310 typo

---

_@JonathanPlasse_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-20 19:34_

---

_Closed by @charliermarsh on 2023-05-20 19:34_

---

_Comment by @github-actions[bot] on 2023-05-20 19:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.19ms     2.8 MB/sec    1.00     14.5±0.17ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.8 MB/sec    1.00      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.8±1.01µs     6.9 MB/sec    1.01    431.9±1.02µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.08ms     4.3 MB/sec    1.00      6.0±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.7±4.56µs    11.3 MB/sec    1.01   1485.3±4.84µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.4±0.31µs    17.9 MB/sec    1.00    165.2±0.32µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.02      5.5±0.03ms     7.4 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1076.4±0.56µs    15.5 MB/sec    1.00   1062.7±0.91µs    15.7 MB/sec
parser/numpy/globals.py                    1.02    110.8±0.15µs    26.6 MB/sec    1.00    108.9±0.30µs    27.1 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    10.9 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.7±0.43ms     2.3 MB/sec    1.00     17.5±0.46ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.21ms     4.1 MB/sec    1.00      4.1±0.23ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   443.1±24.77µs     6.7 MB/sec    1.00   434.3±26.38µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.39ms     3.6 MB/sec    1.00      7.1±0.31ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.24ms     4.7 MB/sec    1.01      8.7±0.22ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1713.0±55.78µs     9.7 MB/sec    1.00  1718.2±75.11µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   185.8±11.63µs    15.9 MB/sec    1.03   190.8±26.83µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.11ms     7.0 MB/sec    1.02      3.7±0.18ms     6.9 MB/sec
parser/large/dataset.py                    1.07      7.3±0.22ms     5.6 MB/sec    1.00      6.8±0.18ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.05  1342.1±43.83µs    12.4 MB/sec    1.00  1283.3±43.04µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    136.3±4.94µs    21.6 MB/sec    1.00   136.1±13.40µs    21.7 MB/sec
parser/pydantic/types.py                   1.05      3.0±0.09ms     8.6 MB/sec    1.00      2.9±0.10ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-05-20 21:46_

---
