```yaml
number: 3669
title: "Allow `pairwise` diagnostics for `zip(..., strict=True)`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/strict
created_at: 2023-03-22T16:51:40Z
updated_at: 2023-03-22T17:23:23Z
url: https://github.com/astral-sh/ruff/pull/3669
synced_at: 2026-01-12T04:39:45Z
```

# Allow `pairwise` diagnostics for `zip(..., strict=True)`

---

_Pull request opened by @charliermarsh on 2023-03-22 16:51_

See: https://github.com/charliermarsh/ruff/pull/3654#discussion_r1144959468.

---

_Label `bug` added by @charliermarsh on 2023-03-22 16:51_

---

_Merged by @charliermarsh on 2023-03-22 17:03_

---

_Closed by @charliermarsh on 2023-03-22 17:03_

---

_Branch deleted on 2023-03-22 17:03_

---

_Comment by @github-actions[bot] on 2023-03-22 17:23_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.02ms     2.9 MB/sec    1.04     14.7±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.02ms     4.4 MB/sec    1.02      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.9±1.82µs     6.8 MB/sec    1.02    444.5±3.14µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.03      6.5±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.06      8.3±0.01ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1712.7±1.84µs     9.7 MB/sec    1.03   1763.7±4.03µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.8±0.74µs    17.0 MB/sec    1.04    181.1±0.78µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec    1.05      3.8±0.00ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.84ms     2.3 MB/sec    1.01     17.4±0.53ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.8±0.24ms     3.5 MB/sec    1.00      4.7±0.20ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   597.1±29.97µs     4.9 MB/sec    1.02   608.2±32.95µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.8±0.46ms     3.3 MB/sec    1.00      7.6±0.34ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      9.7±0.52ms     4.2 MB/sec    1.00      9.5±0.44ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.11ms     8.2 MB/sec    1.07      2.2±0.18ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.7±14.00µs    12.2 MB/sec    1.03   249.5±18.37µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.23ms     5.8 MB/sec    1.02      4.5±0.21ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
