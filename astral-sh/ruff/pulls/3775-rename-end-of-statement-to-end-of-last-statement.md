```yaml
number: 3775
title: "Rename `end_of_statement` to `end_of_last_statement`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/move
created_at: 2023-03-28T16:04:13Z
updated_at: 2023-03-28T16:46:26Z
url: https://github.com/astral-sh/ruff/pull/3775
synced_at: 2026-01-12T04:39:45Z
```

# Rename `end_of_statement` to `end_of_last_statement`

---

_Pull request opened by @charliermarsh on 2023-03-28 16:04_

This function isn't as general as advertised, so I'm moving to a more targeted location, renaming, and documenting appropriately.

Namely, it won't do the right thing given `foo()` in this context:

```py
foo(); bar(
  a,
  b,
)
```

---

_Label `internal` added by @charliermarsh on 2023-03-28 16:04_

---

_Merged by @charliermarsh on 2023-03-28 16:31_

---

_Closed by @charliermarsh on 2023-03-28 16:31_

---

_Branch deleted on 2023-03-28 16:31_

---

_Comment by @github-actions[bot] on 2023-03-28 16:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.02ms     2.9 MB/sec    1.00     13.9±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    469.3±0.86µs     6.3 MB/sec    1.00    467.1±1.99µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.01      6.0±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.6 MB/sec    1.00      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1640.0±4.40µs    10.2 MB/sec    1.00   1638.5±4.32µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    181.7±0.45µs    16.2 MB/sec    1.00    180.7±0.47µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.02ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.3±0.67ms     2.1 MB/sec    1.00     19.1±0.59ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.18ms     3.3 MB/sec    1.00      5.0±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   608.8±31.37µs     4.8 MB/sec    1.01   612.0±28.43µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.36ms     3.0 MB/sec    1.00      8.4±0.39ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02      9.9±0.26ms     4.1 MB/sec    1.00      9.7±0.32ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.8 MB/sec    1.02      2.2±0.09ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    225.8±6.90µs    13.1 MB/sec    1.05   236.8±20.54µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.18ms     5.7 MB/sec    1.03      4.6±0.17ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
