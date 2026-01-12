```yaml
number: 5064
title: Document some attributes on the semantic model
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/document
created_at: 2023-06-13T20:39:06Z
updated_at: 2023-06-13T21:06:31Z
url: https://github.com/astral-sh/ruff/pull/5064
synced_at: 2026-01-12T15:55:17Z
```

# Document some attributes on the semantic model

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-13 20:39_

---

_Merged by @charliermarsh on 2023-06-13 20:45_

---

_Closed by @charliermarsh on 2023-06-13 20:45_

---

_Branch deleted on 2023-06-13 20:45_

---

_Comment by @github-actions[bot] on 2023-06-13 20:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.19ms     5.2 MB/sec    1.12      8.7±0.20ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1640.5±45.14µs    10.2 MB/sec    1.08  1771.6±48.56µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00   163.0±12.06µs    18.1 MB/sec    1.05    171.4±6.38µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.16ms     7.9 MB/sec    1.09      3.5±0.22ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     17.3±0.35ms     2.4 MB/sec    1.02     17.6±0.32ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.10ms     4.0 MB/sec    1.00      4.2±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   535.3±23.72µs     5.5 MB/sec    1.00   534.0±16.91µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.17ms     3.5 MB/sec    1.00      7.4±0.15ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.23ms     4.9 MB/sec    1.00      8.3±0.16ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1779.1±46.12µs     9.4 MB/sec    1.00  1786.3±52.22µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   210.6±10.68µs    14.0 MB/sec    1.00    209.7±6.84µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.8 MB/sec    1.01      3.8±0.11ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      7.4±0.10ms     5.5 MB/sec    1.00      7.0±0.10ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.04  1513.4±27.59µs    11.0 MB/sec    1.00  1454.2±82.75µs    11.5 MB/sec
formatter/numpy/globals.py                 1.03    140.0±4.82µs    21.1 MB/sec    1.00    136.3±3.65µs    21.7 MB/sec
formatter/pydantic/types.py                1.04      3.0±0.06ms     8.4 MB/sec    1.00      2.9±0.08ms     8.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.1±0.36ms     2.7 MB/sec    1.00     14.9±0.22ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.06ms     4.4 MB/sec    1.00      3.8±0.05ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    452.4±9.37µs     6.5 MB/sec    1.00   447.5±11.47µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.12ms     3.9 MB/sec    1.00      6.4±0.12ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.5±0.09ms     5.4 MB/sec    1.00      7.5±0.10ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1584.8±30.18µs    10.5 MB/sec    1.00  1566.9±32.02µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.9±4.22µs    16.9 MB/sec    1.00    174.8±3.95µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.05ms     7.6 MB/sec    1.00      3.3±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
