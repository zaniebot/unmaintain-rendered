```yaml
number: 4499
title: "[`flake8-bandit`] Improve SQL injection detection logic (`S608`)"
type: pull_request
state: merged
author: scop
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/s608-improvements
created_at: 2023-05-18T15:17:02Z
updated_at: 2023-05-18T16:08:43Z
url: https://github.com/astral-sh/ruff/pull/4499
synced_at: 2026-01-12T03:50:03Z
```

# [`flake8-bandit`] Improve SQL injection detection logic (`S608`)

---

_Pull request opened by @scop on 2023-05-18 15:17_

See individual commits for details.

---

_Comment by @github-actions[bot] on 2023-05-18 15:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.01     14.1±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.6±1.03µs     6.9 MB/sec    1.00    425.0±1.00µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.8±4.18µs    11.3 MB/sec    1.00   1468.4±2.62µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.1±0.66µs    17.9 MB/sec    1.00    164.1±1.61µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1062.6±1.08µs    15.7 MB/sec    1.00   1059.3±1.82µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    109.4±0.43µs    27.0 MB/sec    1.00    108.9±1.62µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.01ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.22ms     2.4 MB/sec    1.08     18.0±0.18ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     3.9 MB/sec    1.07      4.5±0.06ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    502.4±9.30µs     5.9 MB/sec    1.00    494.8±7.25µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.12ms     3.6 MB/sec    1.02      7.2±0.31ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1728.3±16.09µs     9.6 MB/sec    1.00  1734.0±22.65µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.8±2.49µs    14.9 MB/sec    1.00    198.3±6.60µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.06ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.01      6.8±0.10ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1285.0±40.94µs    13.0 MB/sec    1.01  1295.8±16.14µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    130.9±2.04µs    22.5 MB/sec    1.02    133.4±2.17µs    22.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.02      2.9±0.03ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-18 15:27_

---

_Closed by @charliermarsh on 2023-05-18 15:27_

---

_Comment by @charliermarsh on 2023-05-18 15:27_

Looks reasonable to me.

---

_Label `rule` added by @charliermarsh on 2023-05-18 15:27_

---

_Renamed from "S608 improvements" to "[`flake8-bandit`] Improve SQL injection detection logic (`S608`)" by @charliermarsh on 2023-05-18 15:50_

---

_Branch deleted on 2023-05-18 16:08_

---
