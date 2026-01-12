```yaml
number: 5757
title: Misc. stylistic changes from flipping through rules late at night
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/misc
created_at: 2023-07-14T05:13:02Z
updated_at: 2023-07-14T05:50:55Z
url: https://github.com/astral-sh/ruff/pull/5757
synced_at: 2026-01-12T15:55:19Z
```

# Misc. stylistic changes from flipping through rules late at night

---

_@charliermarsh_

## Summary

This is really bad PR hygiene, but a mix of: using `Locator`-based fixes in a few places (in lieu of `Generator`-based fixes), using match syntax to avoid `.len() == 1` checks, using common helpers in more places, etc.

## Test Plan

`cargo test`


---

_Merged by @charliermarsh on 2023-07-14 05:23_

---

_Closed by @charliermarsh on 2023-07-14 05:23_

---

_Branch deleted on 2023-07-14 05:23_

---

_Comment by @github-actions[bot] on 2023-07-14 05:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.02ms     5.0 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1865.0±2.21µs     8.9 MB/sec    1.00   1869.1±3.81µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.0±2.13µs    14.1 MB/sec    1.00    208.1±0.21µs    14.2 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.06ms     3.0 MB/sec    1.07     14.8±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.05      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.6±0.84µs     6.5 MB/sec    1.01    461.8±0.60µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.05      6.5±0.02ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.16      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1483.4±1.67µs    11.2 MB/sec    1.11   1643.9±6.11µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.0±0.24µs    17.4 MB/sec    1.06    180.3±0.25µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.12      3.4±0.01ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     12.8±0.36ms     3.2 MB/sec    1.00     12.3±0.49ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.09ms     5.9 MB/sec    1.00      2.8±0.12ms     5.9 MB/sec
formatter/numpy/globals.py                 1.07   331.4±19.51µs     8.9 MB/sec    1.00   308.9±23.49µs     9.6 MB/sec
formatter/pydantic/types.py                1.06      6.2±0.22ms     4.1 MB/sec    1.00      5.8±0.22ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     21.4±0.50ms  1944.9 KB/sec    1.09     23.4±0.49ms  1783.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.20ms     3.0 MB/sec    1.08      5.9±0.23ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   677.6±30.17µs     4.4 MB/sec    1.06   717.0±56.44µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.6±0.32ms     2.6 MB/sec    1.18     11.4±0.91ms     2.2 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.42ms     3.7 MB/sec    1.21     13.3±0.41ms     3.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.6±0.33ms     6.4 MB/sec    1.01      2.6±0.15ms     6.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   282.3±13.96µs    10.5 MB/sec    1.05   296.7±14.08µs     9.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.15ms     5.3 MB/sec    1.15      5.6±0.22ms     4.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
