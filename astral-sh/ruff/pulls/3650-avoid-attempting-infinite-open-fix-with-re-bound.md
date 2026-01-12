```yaml
number: 3650
title: Avoid attempting infinite open fix with re-bound builtin
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/io-open
created_at: 2023-03-21T15:25:41Z
updated_at: 2023-03-21T15:51:17Z
url: https://github.com/astral-sh/ruff/pull/3650
synced_at: 2026-01-12T15:55:13Z
```

# Avoid attempting infinite open fix with re-bound builtin

---

_@charliermarsh_

## Summary

This PR addresses an autofix infinite-loop in which the user imports `io.open` via `from io import open`, which we then attempt to fix by replacing `open` with `open` (intending the latter to be the builtin).

Closes #3647.


---

_Merged by @charliermarsh on 2023-03-21 15:32_

---

_Closed by @charliermarsh on 2023-03-21 15:32_

---

_Branch deleted on 2023-03-21 15:32_

---

_Comment by @github-actions[bot] on 2023-03-21 15:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     17.9±0.67ms     2.3 MB/sec    1.00     17.2±0.52ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.13ms     3.7 MB/sec    1.00      4.4±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.03   630.0±43.86µs     4.7 MB/sec    1.00   611.4±21.41µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.35ms     3.2 MB/sec    1.00      7.8±0.35ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.07      9.5±0.64ms     4.3 MB/sec    1.00      8.9±0.33ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.10ms     8.1 MB/sec    1.00      2.0±0.49ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.1±10.96µs    12.1 MB/sec    1.03   251.4±20.99µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.10ms     6.1 MB/sec    1.01      4.2±0.14ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.05ms     2.7 MB/sec    1.02     15.1±0.48ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    448.6±3.90µs     6.6 MB/sec    1.00    446.7±6.82µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.7±0.47ms     3.8 MB/sec    1.00      6.6±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1750.6±5.44µs     9.5 MB/sec    1.00  1749.6±12.01µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.5±1.25µs    16.3 MB/sec    1.00    181.0±2.83µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
