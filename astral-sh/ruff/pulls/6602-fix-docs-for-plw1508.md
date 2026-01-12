```yaml
number: 6602
title: "Fix docs for `PLW1508`"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/PLW1508
created_at: 2023-08-15T20:08:13Z
updated_at: 2023-08-15T20:45:04Z
url: https://github.com/astral-sh/ruff/pull/6602
synced_at: 2026-01-12T02:52:04Z
```

# Fix docs for `PLW1508`

---

_Pull request opened by @zanieb on 2023-08-15 20:08_

_No description provided._

---

_Label `documentation` added by @zanieb on 2023-08-15 20:08_

---

_Comment by @github-actions[bot] on 2023-08-15 20:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.5±0.04ms     9.1 MB/sec    1.01      4.5±0.03ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.04   937.6±56.18µs    17.8 MB/sec    1.00    901.4±3.89µs    18.5 MB/sec
formatter/numpy/globals.py                 1.00     89.0±0.43µs    33.1 MB/sec    1.04     92.8±4.68µs    31.8 MB/sec
formatter/pydantic/types.py                1.00  1786.7±16.93µs    14.3 MB/sec    1.01  1813.1±62.66µs    14.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.10ms     3.3 MB/sec    1.00     12.5±0.10ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.04ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    467.4±1.30µs     6.3 MB/sec    1.00    466.3±2.40µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.04ms     4.0 MB/sec    1.00      6.5±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.06ms     6.2 MB/sec    1.00      6.5±0.05ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1434.9±3.19µs    11.6 MB/sec    1.00  1437.7±17.46µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.1±0.54µs    17.6 MB/sec    1.00    167.9±1.55µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.7 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.7±0.15ms     8.7 MB/sec    1.00      4.7±0.17ms     8.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   923.8±29.59µs    18.0 MB/sec    1.00   924.3±26.24µs    18.0 MB/sec
formatter/numpy/globals.py                 1.00     89.6±4.10µs    32.9 MB/sec    1.01     90.1±8.13µs    32.8 MB/sec
formatter/pydantic/types.py                1.00  1848.9±60.95µs    13.8 MB/sec    1.00  1855.2±56.94µs    13.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.29ms     2.6 MB/sec    1.00     15.3±0.26ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.13ms     3.9 MB/sec    1.00      4.3±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   550.1±19.46µs     5.4 MB/sec    1.00   544.8±15.71µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.16ms     3.2 MB/sec    1.01      8.0±0.21ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.17ms     4.8 MB/sec    1.01      8.5±0.25ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1803.8±54.76µs     9.2 MB/sec    1.00  1776.7±57.29µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    226.9±8.30µs    13.0 MB/sec    1.00   223.6±10.49µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.09ms     6.7 MB/sec    1.00      3.8±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-08-15 20:25_

---

_Merged by @zanieb on 2023-08-15 20:29_

---

_Closed by @zanieb on 2023-08-15 20:29_

---

_Branch deleted on 2023-08-15 20:29_

---
