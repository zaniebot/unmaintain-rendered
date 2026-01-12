```yaml
number: 4837
title: Fix min-index offset rewrites in F523
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/min-index
created_at: 2023-06-03T20:02:34Z
updated_at: 2023-06-03T20:30:19Z
url: https://github.com/astral-sh/ruff/pull/4837
synced_at: 2026-01-12T15:55:16Z
```

# Fix min-index offset rewrites in F523

---

_@charliermarsh_

## Summary

The logic used here didn't generalize past the _first_ argument in the call being removed (see examples).


---

_@charliermarsh reviewed on 2023-06-03 20:03_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pyflakes/F523.py`:28 on 2023-06-03 20:03_

Previously, this would (wrongly) rewrite to `{0}{2}.format(2, 4)`.

---

_Merged by @charliermarsh on 2023-06-03 20:11_

---

_Closed by @charliermarsh on 2023-06-03 20:11_

---

_Branch deleted on 2023-06-03 20:11_

---

_Comment by @github-actions[bot] on 2023-06-03 20:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.16ms     2.9 MB/sec    1.00     14.2±0.12ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.06ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.4±0.63µs     7.0 MB/sec    1.00    422.7±0.53µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.05ms     4.3 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.03ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1492.9±2.71µs    11.2 MB/sec    1.00   1489.2±4.72µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.6±0.26µs    17.4 MB/sec    1.00    170.0±1.09µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1011.9±2.05µs    16.5 MB/sec    1.00   1012.1±0.49µs    16.5 MB/sec
parser/numpy/globals.py                    1.00    104.4±0.20µs    28.3 MB/sec    1.00    104.8±0.44µs    28.2 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.5 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.27ms     2.4 MB/sec    1.01     17.2±0.61ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.7±8.14µs     5.9 MB/sec    1.00    502.0±7.64µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.10ms     3.6 MB/sec    1.00      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.10ms     4.9 MB/sec    1.01      8.4±0.12ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1782.0±21.26µs     9.3 MB/sec    1.00  1780.5±19.31µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.6±3.35µs    14.5 MB/sec    1.01    205.9±6.64µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.02      3.8±0.18ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.4 MB/sec    1.00      6.4±0.06ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1203.0±13.53µs    13.8 MB/sec    1.01  1217.7±15.29µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    123.5±1.86µs    23.9 MB/sec    1.01    125.2±2.51µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.4 MB/sec    1.02      2.8±0.04ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
