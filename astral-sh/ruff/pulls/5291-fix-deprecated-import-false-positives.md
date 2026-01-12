```yaml
number: 5291
title: "Fix `deprecated-import` false positives"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: fix-up035
created_at: 2023-06-22T08:43:11Z
updated_at: 2023-07-10T09:55:15Z
url: https://github.com/astral-sh/ruff/pull/5291
synced_at: 2026-01-12T03:36:54Z
```

# Fix `deprecated-import` false positives

---

_Pull request opened by @tjkuson on 2023-06-22 08:43_

## Summary

Remove recommendations to replace `typing_extensions.dataclass_transform` and `typing_extensions.SupportsIndex` with their `typing` library counterparts.

Closes #5112.

## Test Plan

Added extra checks to the test fixture.

`cargo test`

---

_Comment by @github-actions[bot] on 2023-06-22 08:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.6±0.28ms     6.2 MB/sec    1.18      7.7±0.20ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1534.5±74.75µs    10.9 MB/sec    1.09  1674.7±57.61µs     9.9 MB/sec
formatter/numpy/globals.py                 1.05    180.7±6.18µs    16.3 MB/sec    1.00   171.4±11.66µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.17ms     7.3 MB/sec    1.01      3.5±0.21ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.20ms     2.6 MB/sec    1.00     15.5±0.22ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.05ms     4.3 MB/sec    1.02      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   474.2±17.39µs     6.2 MB/sec    1.07    505.3±5.66µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.21ms     3.8 MB/sec    1.03      6.9±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.21ms     5.3 MB/sec    1.03      7.9±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1599.5±46.81µs    10.4 MB/sec    1.08  1731.1±25.00µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.5±7.07µs    18.5 MB/sec    1.21    192.2±2.22µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.15ms     8.0 MB/sec    1.12      3.6±0.05ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.8±0.09ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1681.8±20.18µs     9.9 MB/sec    1.00  1683.0±21.54µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    187.3±3.60µs    15.8 MB/sec    1.00    187.4±4.02µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.10ms     6.5 MB/sec    1.00      3.9±0.05ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.17ms     2.6 MB/sec    1.00     15.4±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.13ms     3.9 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.8±6.80µs     5.9 MB/sec    1.03    518.6±8.70µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.15ms     3.7 MB/sec    1.05      7.3±0.31ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.07ms     5.1 MB/sec    1.01      8.0±0.21ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1724.5±23.55µs     9.7 MB/sec    1.03  1767.7±44.40µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.4±3.47µs    15.0 MB/sec    1.00    197.8±3.74µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/pyupgrade/rules/deprecated_import.rs`:280 on 2023-06-22 15:14_

`NamedTuple` has actually been in the typing module since its inception, and `TypedDict` was added in 3.8, so the "Moved in Python 3.11" language in the comment here isn't _strictly_ accurate

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/pyupgrade/rules/deprecated_import.rs`:258 on 2023-06-22 15:15_

`NewType` has also been in the typing module since Python 3.5.2, so the "Moved in Python 3.8" language in the comment is maybe not _quite_ correct

---

_@AlexWaygood reviewed on 2023-06-22 15:17_

Some notes on the comments @charliermarsh (feel free to ignore if you find them too nitpicky ;)

---

_Comment by @charliermarsh on 2023-06-22 15:20_

@AlexWaygood - Not too nit-picky at all -- on the contrary, I really appreciate them!

---

_@AlexWaygood reviewed on 2023-06-22 15:27_

LGTM from what I can tell!

---

_Merged by @charliermarsh on 2023-06-22 15:34_

---

_Closed by @charliermarsh on 2023-06-22 15:34_

---

_Branch deleted on 2023-07-10 09:55_

---
