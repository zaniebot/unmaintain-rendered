```yaml
number: 3639
title: "`pylint`: Implement `binary-op-exception` (`PLW0711`)"
type: pull_request
state: merged
author: latonis
labels: []
assignees: []
merged: true
base: main
head: pylint-binary-op-exception
created_at: 2023-03-21T02:03:04Z
updated_at: 2023-03-21T15:05:09Z
url: https://github.com/astral-sh/ruff/pull/3639
synced_at: 2026-01-12T15:55:13Z
```

# `pylint`: Implement `binary-op-exception` (`PLW0711`)

---

_@latonis_

Rule Reference: https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/binary-op-exception.html

Implemented for #970 

The Pylint documentation only talks about `except E1 or E2`, which only catches the first `exception`. I included a check for any `BoolOp` in the except handler as `except E1 and E2` will run in Python as well, and catch the last in the statement.

---

_Comment by @github-actions[bot] on 2023-03-21 02:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.5±0.56ms     2.3 MB/sec    1.00     17.4±0.58ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.25ms     3.7 MB/sec    1.00      4.5±0.25ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   625.0±36.47µs     4.7 MB/sec    1.00   627.5±44.87µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.8±0.31ms     3.3 MB/sec    1.00      7.7±0.35ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.04      9.7±0.47ms     4.2 MB/sec    1.00      9.3±0.40ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.10ms     7.8 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   252.3±15.20µs    11.7 MB/sec    1.06   267.8±21.41µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.17ms     5.7 MB/sec    1.06      4.7±0.33ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.11ms     2.7 MB/sec    1.00     14.9±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   541.9±10.86µs     5.4 MB/sec    1.00    531.1±7.52µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.9±0.19ms     3.7 MB/sec    1.00      6.7±0.04ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.11ms     4.9 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1863.3±28.67µs     8.9 MB/sec    1.00  1829.0±61.39µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.3±4.13µs    14.7 MB/sec    1.03   205.9±11.64µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.07ms     6.6 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-21 03:33_

---

_Closed by @charliermarsh on 2023-03-21 03:33_

---

_Branch deleted on 2023-03-21 15:05_

---
