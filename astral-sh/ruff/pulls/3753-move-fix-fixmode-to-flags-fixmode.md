```yaml
number: 3753
title: "Move `fix::FixMode` to `flags::FixMode`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fix-mod
created_at: 2023-03-26T21:34:59Z
updated_at: 2023-03-26T22:03:24Z
url: https://github.com/astral-sh/ruff/pull/3753
synced_at: 2026-01-12T15:55:13Z
```

# Move `fix::FixMode` to `flags::FixMode`

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-03-26 21:34_

---

_Renamed from "Move fix::FixMode to flags::FixMode" to "Move `fix::FixMode` to `flags::FixMode`" by @charliermarsh on 2023-03-26 21:35_

---

_Merged by @charliermarsh on 2023-03-26 21:40_

---

_Closed by @charliermarsh on 2023-03-26 21:40_

---

_Branch deleted on 2023-03-26 21:40_

---

_Comment by @github-actions[bot] on 2023-03-26 21:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.06ms     3.0 MB/sec    1.00     13.7±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.9±1.28µs     5.9 MB/sec    1.00    497.6±1.18µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.04ms     5.5 MB/sec    1.00      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1657.8±3.49µs    10.0 MB/sec    1.00   1656.4±1.56µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.4±1.20µs    16.3 MB/sec    1.03    186.4±0.47µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.4 MB/sec    1.00      3.4±0.01ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.11ms     2.7 MB/sec    1.00     15.0±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    458.8±3.59µs     6.4 MB/sec    1.00    452.0±5.53µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.02ms     3.8 MB/sec    1.00      6.7±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.00      8.1±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1750.6±6.30µs     9.5 MB/sec    1.01  1772.7±65.99µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.4±1.40µs    16.1 MB/sec    1.00    184.0±3.72µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
