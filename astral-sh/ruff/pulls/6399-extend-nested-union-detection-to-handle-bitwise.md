```yaml
number: 6399
title: "Extend nested union detection to handle bitwise or `Union` expressions"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/nested-unions
created_at: 2023-08-07T18:19:11Z
updated_at: 2023-08-07T19:17:28Z
url: https://github.com/astral-sh/ruff/pull/6399
synced_at: 2026-01-12T02:52:04Z
```

# Extend nested union detection to handle bitwise or `Union` expressions

---

_Pull request opened by @charliermarsh on 2023-08-07 18:19_

## Summary

We have some logic in the expression analyzer method to avoid re-checking the inner `Union` in `Union[Union[...]]`, since the methods that analyze `Union` expressions already recurse. Elsewhere, we have logic to avoid re-checking the inner `|` in `int | (int | str)`, for the same reason.

This PR unifies that logic into a single method _and_ ensures that, just as we recurse over both `Union` and `|`, we also detect that we're in _either_ kind of nested union.

Closes https://github.com/astral-sh/ruff/issues/6285.

## Test Plan

Added some new snapshots.


---

_@charliermarsh reviewed on 2023-08-07 18:19_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.py`:86 on 2023-08-07 18:19_

(This file should be in-sync with `crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.pyi`, the edits here are just copying over `crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.pyi`, I recommend ignoring this file and its snapshot changes.)

---

_@charliermarsh reviewed on 2023-08-07 18:20_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.pyi`:81 on 2023-08-07 18:20_

Prior to https://github.com/astral-sh/ruff/pull/6345, we emitted three times here (incorrectly). See: https://github.com/astral-sh/ruff/issues/6285.

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.pyi`:86 on 2023-08-07 18:20_

Prior to adding `in_nested_union` in this PR, we emitted duplicate violations here.

---

_@charliermarsh reviewed on 2023-08-07 18:20_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-07 18:20_

---

_Comment by @github-actions[bot] on 2023-08-07 18:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      8.6±0.47ms     4.7 MB/sec     1.00      8.5±0.60ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1675.2±111.68µs     9.9 MB/sec    1.02  1712.1±114.00µs     9.7 MB/sec
formatter/numpy/globals.py                 1.08   207.0±12.57µs    14.3 MB/sec     1.00   190.9±14.83µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.26ms     7.0 MB/sec     1.03      3.8±0.37ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.06     11.7±0.70ms     3.5 MB/sec     1.00     11.1±0.51ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.14ms     5.5 MB/sec     1.10      3.3±0.19ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   426.2±26.69µs     6.9 MB/sec     1.01   429.2±29.36µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.2±0.28ms     4.9 MB/sec     1.07      5.5±0.33ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.05      5.9±0.42ms     6.9 MB/sec     1.00      5.7±0.24ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1276.4±96.27µs    13.0 MB/sec     1.00  1211.2±62.83µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.07   168.9±11.82µs    17.5 MB/sec     1.00    157.3±8.65µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.8±0.14ms     9.2 MB/sec     1.00      2.7±0.21ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.23ms     4.1 MB/sec    1.00      9.9±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1890.5±15.42µs     8.8 MB/sec    1.00  1884.6±14.58µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    192.3±2.84µs    15.3 MB/sec    1.01    194.6±5.92µs    15.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.0 MB/sec    1.00      4.2±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.04     13.2±0.22ms     3.1 MB/sec    1.00     12.7±0.09ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.03ms     4.6 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.1±4.94µs     8.1 MB/sec    1.00    364.6±4.76µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.07ms     4.2 MB/sec    1.00      5.9±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.07ms     5.8 MB/sec    1.00      6.9±0.10ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1424.3±15.11µs    11.7 MB/sec    1.00  1391.3±13.21µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    143.0±1.70µs    20.6 MB/sec    1.00    141.1±1.33µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.1±0.03ms     8.2 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-07 18:41_

---

_Merged by @charliermarsh on 2023-08-07 19:17_

---

_Closed by @charliermarsh on 2023-08-07 19:17_

---

_Branch deleted on 2023-08-07 19:17_

---
