```yaml
number: 5273
title: "Ignore Pydantic classes when evaluating `mutable-class-default` (`RUF012`) "
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pydantic
created_at: 2023-06-21T23:47:05Z
updated_at: 2023-10-25T08:43:01Z
url: https://github.com/astral-sh/ruff/pull/5273
synced_at: 2026-01-12T15:55:18Z
```

# Ignore Pydantic classes when evaluating `mutable-class-default` (`RUF012`) 

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/5272.

---

_Label `bug` added by @charliermarsh on 2023-06-21 23:47_

---

_Renamed from "Ignore Pydantic classes in RUF012" to "Ignore Pydantic classes when evaluating `mutable-class-default` (`RUF012`) " by @charliermarsh on 2023-06-21 23:47_

---

_Merged by @charliermarsh on 2023-06-21 23:59_

---

_Closed by @charliermarsh on 2023-06-21 23:59_

---

_Branch deleted on 2023-06-21 23:59_

---

_Comment by @github-actions[bot] on 2023-06-22 00:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.10ms     5.4 MB/sec    1.01      7.6±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1625.3±25.54µs    10.2 MB/sec    1.00  1630.9±25.21µs    10.2 MB/sec
formatter/numpy/globals.py                 1.01    153.3±3.79µs    19.2 MB/sec    1.00    152.5±2.98µs    19.3 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.06ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.23ms     2.7 MB/sec    1.00     15.1±0.16ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.05ms     4.3 MB/sec    1.00      3.8±0.06ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.7±8.56µs     6.0 MB/sec    1.00    493.1±7.56µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.10ms     3.8 MB/sec    1.01      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.09ms     5.3 MB/sec    1.00      7.6±0.09ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1676.4±26.22µs     9.9 MB/sec    1.00  1679.0±30.06µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.4±3.66µs    15.7 MB/sec    1.00    187.9±3.71µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.05ms     7.3 MB/sec    1.00      3.5±0.04ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.08ms     5.2 MB/sec    1.00      7.8±0.12ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1658.3±37.51µs    10.0 MB/sec    1.00  1641.7±33.11µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    159.2±3.81µs    18.5 MB/sec    1.01   161.2±13.35µs    18.3 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.05ms     7.8 MB/sec    1.00      3.3±0.05ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.20ms     2.6 MB/sec    1.02     16.0±0.59ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   510.3±11.83µs     5.8 MB/sec    1.00    503.6±8.40µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.11ms     3.6 MB/sec    1.00      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.7±47.35µs     9.5 MB/sec    1.00  1749.6±20.43µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    203.3±7.75µs    14.5 MB/sec    1.00    201.6±3.48µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @9128305 on 2023-06-22 18:46_

It does not work for `BaseSettings` (https://docs.pydantic.dev/latest/usage/settings/)

---

_Comment by @charliermarsh on 2023-06-22 19:14_

Thanks, tracking: https://github.com/astral-sh/ruff/issues/5308.

---

_Comment by @sudrich on 2023-10-24 13:01_

Seem to only for direct and not transitive inheritance, which seems like an edge case not considered yet.

Nevermind, say it's already being tracked by #5639.

---
