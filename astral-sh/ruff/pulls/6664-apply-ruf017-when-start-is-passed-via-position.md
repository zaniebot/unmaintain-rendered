```yaml
number: 6664
title: Apply RUF017 when start is passed via position
type: pull_request
state: merged
author: hauntsaninja
labels:
  - rule
assignees: []
merged: true
base: main
head: ruf017-fix
created_at: 2023-08-18T00:00:01Z
updated_at: 2023-08-18T02:17:37Z
url: https://github.com/astral-sh/ruff/pull/6664
synced_at: 2026-01-12T02:52:04Z
```

# Apply RUF017 when start is passed via position

---

_Pull request opened by @hauntsaninja on 2023-08-18 00:00_

As discussed in https://github.com/astral-sh/ruff/pull/6489#discussion_r1297858919. Linking https://github.com/astral-sh/ruff/issues/5073

---

_@charliermarsh approved on 2023-08-18 00:10_

Thanks so much @hauntsaninja for noticing this and for following up with the PR, much appreciated.

---

_Merged by @charliermarsh on 2023-08-18 00:10_

---

_Closed by @charliermarsh on 2023-08-18 00:10_

---

_Label `rule` added by @charliermarsh on 2023-08-18 00:10_

---

_Comment by @github-actions[bot] on 2023-08-18 00:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.5±0.02ms    11.8 MB/sec    1.00      3.4±0.02ms    11.9 MB/sec
formatter/numpy/ctypeslib.py               1.00    715.0±4.20µs    23.3 MB/sec    1.00    712.8±2.22µs    23.4 MB/sec
formatter/numpy/globals.py                 1.01     76.8±0.29µs    38.4 MB/sec    1.00     76.4±1.03µs    38.6 MB/sec
formatter/pydantic/types.py                1.02  1404.5±20.71µs    18.2 MB/sec    1.00  1381.1±32.55µs    18.5 MB/sec
linter/all-rules/large/dataset.py          1.02     10.4±0.10ms     3.9 MB/sec    1.00     10.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.02ms     6.0 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.2±0.49µs     7.6 MB/sec    1.00    388.5±0.90µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.05ms     4.8 MB/sec    1.00      5.3±0.01ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.5 MB/sec    1.01      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1203.6±1.57µs    13.8 MB/sec    1.00   1202.2±5.53µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.4±1.12µs    21.2 MB/sec    1.01    140.6±0.58µs    21.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.00ms    10.4 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.05ms    10.8 MB/sec    1.01      3.8±0.06ms    10.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   774.7±10.88µs    21.5 MB/sec    1.01   779.5±19.84µs    21.4 MB/sec
formatter/numpy/globals.py                 1.00     81.2±1.72µs    36.4 MB/sec    1.01     82.0±1.78µs    36.0 MB/sec
formatter/pydantic/types.py                1.00  1545.9±30.54µs    16.5 MB/sec    1.02  1574.3±54.30µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.16ms     3.2 MB/sec    1.01     12.8±0.14ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.0±5.17µs     6.8 MB/sec    1.00    437.5±4.71µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.10ms     3.8 MB/sec    1.00      6.6±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.00      7.0±0.11ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1484.1±17.17µs    11.2 MB/sec    1.00  1486.6±16.47µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.5±2.46µs    16.9 MB/sec    1.00    175.0±2.27µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.06ms     8.2 MB/sec    1.00      3.1±0.03ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-08-18 00:13_

---

_Comment by @evanrittenhouse on 2023-08-18 02:17_

Thanks @hauntsaninja!

---
