```yaml
number: 4385
title: "Ignore some methods on list in `flake8-boolean-trap`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/boolean-trap
created_at: 2023-05-11T21:49:54Z
updated_at: 2023-05-11T22:29:53Z
url: https://github.com/astral-sh/ruff/pull/4385
synced_at: 2026-01-12T03:56:39Z
```

# Ignore some methods on list in `flake8-boolean-trap`

---

_Pull request opened by @charliermarsh on 2023-05-11 21:49_

Closes #4382.

---

_Marked ready for review by @charliermarsh on 2023-05-11 21:49_

---

_Merged by @charliermarsh on 2023-05-11 21:55_

---

_Closed by @charliermarsh on 2023-05-11 21:55_

---

_Branch deleted on 2023-05-11 21:55_

---

_Comment by @github-actions[bot] on 2023-05-11 21:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.7±0.38ms     2.3 MB/sec    1.00     17.8±0.51ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.20ms     3.9 MB/sec    1.00      4.2±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   533.5±13.91µs     5.5 MB/sec    1.00   529.2±15.44µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.21ms     3.4 MB/sec    1.00      7.3±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.27ms     4.8 MB/sec    1.00      8.4±0.16ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1795.8±40.49µs     9.3 MB/sec    1.00  1777.5±97.37µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    212.8±5.84µs    13.9 MB/sec    1.02   216.7±10.19µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.08ms     6.7 MB/sec    1.00      3.7±0.08ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.21ms     6.0 MB/sec    1.01      6.9±0.22ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1320.8±49.83µs    12.6 MB/sec    1.02  1342.8±37.12µs    12.4 MB/sec
parser/numpy/globals.py                    1.00    132.2±4.53µs    22.3 MB/sec    1.00    132.2±4.03µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.17ms     8.8 MB/sec    1.01      2.9±0.12ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.7±0.16ms     2.4 MB/sec    1.00     16.6±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.9±6.07µs     6.0 MB/sec    1.00    488.5±5.74µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.07ms     3.6 MB/sec    1.00      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.4±0.07ms     4.8 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1757.0±17.95µs     9.5 MB/sec    1.00  1725.8±17.91µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.8±3.66µs    15.2 MB/sec    1.02    196.8±5.37µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.07ms     6.8 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.2 MB/sec    1.01      6.7±0.05ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1252.2±14.51µs    13.3 MB/sec    1.01  1259.8±13.53µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    127.6±2.53µs    23.1 MB/sec    1.01    128.9±1.93µs    22.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
