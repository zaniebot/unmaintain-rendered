```yaml
number: 6453
title: Bump version to 0.0.284
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: release/284
created_at: 2023-08-09T17:45:32Z
updated_at: 2023-08-09T18:32:35Z
url: https://github.com/astral-sh/ruff/pull/6453
synced_at: 2026-01-12T15:55:21Z
```

# Bump version to 0.0.284

---

_@zanieb_

## What's Changed

This release fixes a few bugs, notably the previous release announced a breaking change where the default target
Python version changed from 3.10 to 3.8 but it was not applied. Thanks to @rco-ableton for fixing this in 
https://github.com/astral-sh/ruff/pull/6444

### Bug Fixes
* Do not trigger `S108` if path is inside `tempfile.*` call by @dhruvmanila in https://github.com/astral-sh/ruff/pull/6416
* Do not allow on zero tab width by @tjkuson in https://github.com/astral-sh/ruff/pull/6429
* Fix false-positive in submodule resolution by @charliermarsh in https://github.com/astral-sh/ruff/pull/6435

## New Contributors
* @rco-ableton made their first contribution in https://github.com/astral-sh/ruff/pull/6444

**Full Changelog**: https://github.com/astral-sh/ruff/compare/v0.0.283...v0.0.284

---

_Comment by @github-actions[bot] on 2023-08-09 18:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.2±0.11ms     4.9 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1653.7±54.61µs    10.1 MB/sec    1.00  1646.1±30.24µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    189.6±7.67µs    15.6 MB/sec    1.01    191.7±7.38µs    15.4 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.15ms     7.2 MB/sec    1.00      3.5±0.11ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.01     10.1±0.02ms     4.0 MB/sec    1.00     10.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.05ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.1±1.48µs     7.7 MB/sec    1.00    384.8±1.84µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.03ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.01ms     7.6 MB/sec    1.00      5.3±0.01ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1155.8±12.50µs    14.4 MB/sec    1.00  1156.9±14.80µs    14.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    135.9±3.99µs    21.7 MB/sec    1.00    135.3±5.85µs    21.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.6 MB/sec    1.00      2.4±0.02ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.3±0.30ms     3.3 MB/sec    1.00     12.1±0.25ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.07ms     7.1 MB/sec    1.00      2.3±0.09ms     7.2 MB/sec
formatter/numpy/globals.py                 1.01   262.6±38.28µs    11.2 MB/sec    1.00   260.1±13.75µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.16ms     5.0 MB/sec    1.03      5.2±0.19ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.30ms     2.7 MB/sec    1.01     15.5±0.29ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     4.0 MB/sec    1.00      4.2±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   518.1±17.39µs     5.7 MB/sec    1.03   532.2±26.41µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.1±0.18ms     3.1 MB/sec    1.00      7.9±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.17ms     4.8 MB/sec    1.00      8.4±0.15ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1733.7±48.80µs     9.6 MB/sec    1.02  1759.9±64.51µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   199.3±11.99µs    14.8 MB/sec    1.00   199.1±10.96µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.15ms     6.9 MB/sec    1.00      3.7±0.11ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-09 18:17_

---

_Merged by @zanieb on 2023-08-09 18:32_

---

_Closed by @zanieb on 2023-08-09 18:32_

---

_Branch deleted on 2023-08-09 18:32_

---
