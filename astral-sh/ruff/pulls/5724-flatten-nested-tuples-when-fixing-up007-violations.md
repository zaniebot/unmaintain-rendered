```yaml
number: 5724
title: Flatten nested tuples when fixing UP007 violations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/flat
created_at: 2023-07-13T04:03:24Z
updated_at: 2023-07-13T04:28:46Z
url: https://github.com/astral-sh/ruff/pull/5724
synced_at: 2026-01-12T15:55:19Z
```

# Flatten nested tuples when fixing UP007 violations

---

_@charliermarsh_

## Summary

Also upgrading these to "Suggested" from "Manual" (they should've always been "Suggested", I think), and adding some more test cases.

---

_Merged by @charliermarsh on 2023-07-13 04:11_

---

_Closed by @charliermarsh on 2023-07-13 04:11_

---

_Branch deleted on 2023-07-13 04:11_

---

_Comment by @github-actions[bot] on 2023-07-13 04:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1876.9±1.36µs     8.9 MB/sec    1.00   1875.0±3.32µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.0±0.33µs    14.1 MB/sec    1.00    209.4±1.15µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.07ms     3.0 MB/sec    1.00     13.6±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    431.2±0.91µs     6.8 MB/sec    1.00    427.8±0.73µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1462.6±4.09µs    11.4 MB/sec    1.00   1459.1±3.50µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.1±0.25µs    17.8 MB/sec    1.00    165.3±0.26µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.9±0.55ms     3.4 MB/sec    1.00     11.6±0.42ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.6±0.16ms     6.3 MB/sec    1.00      2.6±0.12ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   298.8±14.83µs     9.9 MB/sec    1.06   316.5±15.41µs     9.3 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.22ms     4.6 MB/sec    1.04      5.7±0.28ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.7±0.95ms     2.1 MB/sec    1.00     19.7±0.89ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.17ms     3.4 MB/sec    1.03      5.0±0.18ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   604.1±24.46µs     4.9 MB/sec    1.00   602.7±21.33µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.26ms     3.1 MB/sec    1.02      8.5±0.32ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.29ms     4.2 MB/sec    1.01      9.8±0.29ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     8.1 MB/sec    1.02      2.1±0.08ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   246.2±10.06µs    12.0 MB/sec    1.04   257.2±16.22µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.16ms     5.9 MB/sec    1.02      4.5±0.19ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
