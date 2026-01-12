```yaml
number: 6368
title: "Revert change to `require_git(false)` in `WalkBuilder`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/gitignore
created_at: 2023-08-05T19:35:46Z
updated_at: 2023-08-05T20:09:13Z
url: https://github.com/astral-sh/ruff/pull/6368
synced_at: 2026-01-12T02:52:04Z
```

# Revert change to `require_git(false)` in `WalkBuilder`

---

_Pull request opened by @charliermarsh on 2023-08-05 19:35_

## Summary

This was changed to fix https://github.com/astral-sh/ruff/issues/5930 (respect `.gitignore` for unzipped source repositories), but led to undesirable behavior whereby `.gitignore` files in parent directories are respected regardless of whether you're working in a child git repository (see: https://github.com/astral-sh/ruff/issues/6335). The latter is a bigger problem than the former is an important use-case to support, so pragmatically erring on the side of a revert.

Closes https://github.com/astral-sh/ruff/issues/6335.

---

_Merged by @charliermarsh on 2023-08-05 19:45_

---

_Closed by @charliermarsh on 2023-08-05 19:45_

---

_Branch deleted on 2023-08-05 19:45_

---

_Comment by @github-actions[bot] on 2023-08-05 19:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.24ms     5.3 MB/sec    1.00      7.7±0.18ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1546.9±56.07µs    10.8 MB/sec    1.00  1536.5±45.31µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    174.6±9.03µs    16.9 MB/sec    1.03   180.2±11.03µs    16.4 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.09ms     7.9 MB/sec    1.01      3.3±0.12ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.39ms     4.1 MB/sec    1.08     10.8±0.57ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.6±0.06ms     6.3 MB/sec    1.02      2.7±0.11ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   372.0±13.89µs     7.9 MB/sec    1.01   374.3±11.60µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.5±0.14ms     5.6 MB/sec    1.04      4.7±0.13ms     5.4 MB/sec
linter/default-rules/large/dataset.py      1.00      5.0±0.16ms     8.1 MB/sec    1.00      5.0±0.12ms     8.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1088.8±29.98µs    15.3 MB/sec    1.02  1109.8±49.33µs    15.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    122.5±5.82µs    24.1 MB/sec    1.01    123.7±6.25µs    23.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.06ms    11.3 MB/sec    1.01      2.3±0.07ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.50ms     3.9 MB/sec    1.00     10.4±0.46ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.12ms     8.3 MB/sec    1.00      2.0±0.13ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   216.8±15.23µs    13.6 MB/sec    1.04   225.5±22.52µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.23ms     5.9 MB/sec    1.03      4.4±0.31ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.41ms     3.0 MB/sec    1.07     14.4±0.69ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.15ms     4.6 MB/sec    1.02      3.7±0.19ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   451.5±28.45µs     6.5 MB/sec    1.00   447.0±24.03µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.46ms     4.1 MB/sec    1.04      6.5±0.29ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.25ms     5.6 MB/sec    1.00      7.2±0.26ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1483.8±74.70µs    11.2 MB/sec    1.01  1493.5±84.51µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   174.0±14.43µs    17.0 MB/sec    1.02   177.2±19.66µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.16ms     7.9 MB/sec    1.02      3.3±0.20ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
