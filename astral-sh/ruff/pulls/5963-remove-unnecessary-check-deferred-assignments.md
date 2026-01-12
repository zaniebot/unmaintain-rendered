```yaml
number: 5963
title: "Remove unnecessary `check_deferred_assignments`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/visit
created_at: 2023-07-22T01:50:04Z
updated_at: 2023-07-22T02:34:28Z
url: https://github.com/astral-sh/ruff/pull/5963
synced_at: 2026-01-12T03:30:22Z
```

# Remove unnecessary `check_deferred_assignments`

---

_Pull request opened by @charliermarsh on 2023-07-22 01:50_

## Summary

These rules can just be included in the `check_deferred_scopes`.


---

_Marked ready for review by @charliermarsh on 2023-07-22 01:50_

---

_@charliermarsh reviewed on 2023-07-22 01:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__augmented_assignment_after_del.snap`:19 on 2023-07-22 01:50_

(This is just a tiebreaker, i.e., the diagnostics are reordered.)

---

_Merged by @charliermarsh on 2023-07-22 02:08_

---

_Closed by @charliermarsh on 2023-07-22 02:08_

---

_Branch deleted on 2023-07-22 02:08_

---

_Comment by @github-actions[bot] on 2023-07-22 02:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.17ms     4.3 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1848.3±8.09µs     9.0 MB/sec    1.00  1843.9±12.80µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    201.5±2.26µs    14.6 MB/sec    1.00    201.6±1.91µs    14.6 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.02ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.16ms     3.1 MB/sec    1.00     13.0±0.08ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.4±1.14µs     6.8 MB/sec    1.00    437.3±1.72µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.2 MB/sec    1.01      6.6±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1417.4±3.21µs    11.7 MB/sec    1.01   1427.5±1.51µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.9±0.27µs    18.5 MB/sec    1.00    160.4±1.00µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.08ms     3.7 MB/sec    1.01     11.2±0.06ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.02      2.2±0.02ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    231.7±2.72µs    12.7 MB/sec    1.01    235.0±5.07µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.4 MB/sec    1.02      4.8±0.04ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.10ms     2.6 MB/sec    1.02     15.8±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.02      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   425.2±10.14µs     6.9 MB/sec    1.03    439.8±6.47µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.06ms     3.6 MB/sec    1.04      7.3±0.07ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.02      8.3±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1659.6±16.19µs    10.0 MB/sec    1.02  1689.0±13.05µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.9±1.50µs    16.8 MB/sec    1.01    177.4±2.98µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.01      3.7±0.02ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
