```yaml
number: 5518
title: "Refactor the `unnecessary-map` (`C417`) implementation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/c417
created_at: 2023-07-05T00:10:45Z
updated_at: 2023-07-05T00:38:03Z
url: https://github.com/astral-sh/ruff/pull/5518
synced_at: 2026-01-12T15:55:18Z
```

# Refactor the `unnecessary-map` (`C417`) implementation

---

_@charliermarsh_

## Summary

No behavioral changes. Just refactors + adding a test for a false positive, which I'll fix in a downstream PR.

---

_Label `internal` added by @charliermarsh on 2023-07-05 00:13_

---

_Label `autofix` added by @charliermarsh on 2023-07-05 00:13_

---

_Label `autofix` removed by @charliermarsh on 2023-07-05 00:13_

---

_Comment by @github-actions[bot] on 2023-07-05 00:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.04ms     5.2 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1746.4±1.46µs     9.5 MB/sec    1.00   1733.4±2.43µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    193.5±0.28µs    15.2 MB/sec    1.00    192.8±0.19µs    15.3 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.07     14.3±0.04ms     2.8 MB/sec    1.00     13.4±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.00ms     4.8 MB/sec    1.00      3.4±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    443.2±0.57µs     6.7 MB/sec    1.00    433.1±0.55µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.2±0.04ms     4.1 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.13      7.6±0.01ms     5.3 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10   1626.0±2.87µs    10.2 MB/sec    1.00   1484.4±1.49µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.06    181.7±0.69µs    16.2 MB/sec    1.00    171.2±0.59µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.10      3.4±0.00ms     7.5 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1973.0±14.37µs     8.4 MB/sec    1.00  1970.0±14.80µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    217.5±1.54µs    13.6 MB/sec    1.02   221.1±11.06µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.04ms     5.7 MB/sec    1.00      4.4±0.03ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.06ms     2.6 MB/sec    1.00     15.4±0.06ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.03ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    430.0±5.12µs     6.9 MB/sec    1.00    426.2±4.38µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.02ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.02ms     5.0 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1671.8±8.49µs    10.0 MB/sec    1.00  1651.5±13.66µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.6±0.82µs    16.3 MB/sec    1.00    179.5±1.15µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-05 00:25_

---

_Closed by @charliermarsh on 2023-07-05 00:25_

---

_Branch deleted on 2023-07-05 00:25_

---
