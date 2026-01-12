```yaml
number: 6045
title: Perform lint rule analysis after subtree traversal
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/reorder-steps
created_at: 2023-07-24T21:51:31Z
updated_at: 2023-07-25T13:05:46Z
url: https://github.com/astral-sh/ruff/pull/6045
synced_at: 2026-01-12T15:55:20Z
```

# Perform lint rule analysis after subtree traversal

---

_@charliermarsh_

## Summary

This PR modifies the order of operations in our AST checker. Previously, we ran our analysis rules first, then bound names and traversed over the subtrees. Now, after a series of refactors, we can invert the order: do the subtree traversal and model-building _first_, then run rules.

The nice thing about this change is that when we go to analyze, e.g., a function call node, we'll already have traversed any of the constituent `Expr::Name` nodes... So if we store the resolution of all names when do the traversal, we can avoid having to do any expensive work in `resolve_call_path`.

## Test Plan

Clean run of the snapshot tests, and hopefully the ecosystem checks too!


---

_@charliermarsh reviewed on 2023-07-24 21:51_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_future_annotations/snapshots/ruff__rules__flake8_future_annotations__tests__fa102_no_future_import_uses_union.py.snap`:30 on 2023-07-24 21:51_

(This is just a re-ordering of diagnostics that occur in the same position.)

---

_Comment by @github-actions[bot] on 2023-07-24 22:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.1±0.26ms     4.5 MB/sec    1.03      9.4±0.34ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1851.1±84.19µs     9.0 MB/sec    1.03  1908.3±82.39µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00   215.6±11.82µs    13.7 MB/sec    1.01   218.2±14.71µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.17ms     6.4 MB/sec    1.03      4.1±0.19ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.39ms     3.1 MB/sec    1.01     13.2±0.37ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.13ms     5.0 MB/sec    1.01      3.4±0.15ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   450.9±25.70µs     6.5 MB/sec    1.01   456.3±28.98µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.30ms     4.3 MB/sec    1.01      6.0±0.22ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.18ms     6.1 MB/sec    1.01      6.7±0.19ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1441.0±51.13µs    11.6 MB/sec    1.01  1450.1±49.41µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.5±8.30µs    17.2 MB/sec    1.02    175.7±9.61µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.12ms     8.3 MB/sec    1.01      3.1±0.12ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.2±0.35ms     3.6 MB/sec    1.00     10.9±0.24ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.06ms     7.6 MB/sec    1.00      2.2±0.07ms     7.7 MB/sec
formatter/numpy/globals.py                 1.01    248.1±7.42µs    11.9 MB/sec    1.00   245.4±11.92µs    12.0 MB/sec
formatter/pydantic/types.py                1.02      4.8±0.13ms     5.3 MB/sec    1.00      4.7±0.10ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.36ms     2.7 MB/sec    1.03     15.6±0.56ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.07ms     4.2 MB/sec    1.03      4.1±0.16ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   482.7±13.94µs     6.1 MB/sec    1.00   484.8±15.46µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.10ms     3.7 MB/sec    1.04      7.2±0.32ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.17ms     5.1 MB/sec    1.03      8.2±0.26ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1711.1±34.49µs     9.7 MB/sec    1.00  1716.8±54.53µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.1±7.47µs    14.9 MB/sec    1.00    198.0±7.82µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.07ms     7.0 MB/sec    1.01      3.7±0.07ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-25 06:40_

---

_Merged by @charliermarsh on 2023-07-25 13:05_

---

_Closed by @charliermarsh on 2023-07-25 13:05_

---

_Branch deleted on 2023-07-25 13:05_

---
