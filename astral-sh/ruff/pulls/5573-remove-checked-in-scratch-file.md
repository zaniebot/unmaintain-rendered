```yaml
number: 5573
title: Remove checked-in scratch file
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/foo
created_at: 2023-07-06T21:37:34Z
updated_at: 2023-07-07T10:08:49Z
url: https://github.com/astral-sh/ruff/pull/5573
synced_at: 2026-01-12T03:36:55Z
```

# Remove checked-in scratch file

---

_Pull request opened by @charliermarsh on 2023-07-06 21:37_

_No description provided._

---

_@zanieb approved on 2023-07-06 21:38_

---

_Merged by @charliermarsh on 2023-07-06 21:46_

---

_Closed by @charliermarsh on 2023-07-06 21:46_

---

_Branch deleted on 2023-07-06 21:46_

---

_Comment by @github-actions[bot] on 2023-07-06 21:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1744.5±2.32µs     9.5 MB/sec    1.00   1746.1±2.95µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    196.1±0.47µs    15.1 MB/sec    1.00    196.7±0.20µs    15.0 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.02ms     3.0 MB/sec    1.00     13.4±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.8±0.36µs     6.8 MB/sec    1.00    435.9±0.49µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1484.8±5.34µs    11.2 MB/sec    1.00   1479.0±2.33µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    170.3±0.30µs    17.3 MB/sec    1.00    169.4±0.64µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.31ms     3.4 MB/sec    1.01     12.2±0.27ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.07ms     6.5 MB/sec    1.02      2.6±0.22ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   287.6±14.84µs    10.3 MB/sec    1.03   296.6±31.51µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.16ms     4.5 MB/sec    1.02      5.8±0.17ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.02     19.4±0.34ms     2.1 MB/sec    1.00     19.1±0.33ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.1±0.22ms     3.2 MB/sec    1.00      5.0±0.12ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   609.7±19.83µs     4.8 MB/sec    1.00   607.3±15.13µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.19ms     3.1 MB/sec    1.02      8.5±0.19ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.19ms     4.2 MB/sec    1.00      9.8±0.23ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     8.0 MB/sec    1.00      2.1±0.06ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    244.9±9.21µs    12.0 MB/sec    1.00    243.0±9.49µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.14ms     5.8 MB/sec    1.00      4.4±0.35ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-07-07 10:08_

btw i've put `scratch.py` into gitignore, that's my way of avoiding checking in a scratch file

---
