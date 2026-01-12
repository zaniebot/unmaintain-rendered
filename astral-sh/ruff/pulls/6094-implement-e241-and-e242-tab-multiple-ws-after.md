```yaml
number: 6094
title: Implement E241 and E242 (tab/multiple ws after commas)
type: pull_request
state: merged
author: akx
labels:
  - rule
assignees: []
merged: true
base: main
head: e241-e242
created_at: 2023-07-26T12:55:40Z
updated_at: 2023-07-27T19:21:07Z
url: https://github.com/astral-sh/ruff/pull/6094
synced_at: 2026-01-12T03:30:22Z
```

# Implement E241 and E242 (tab/multiple ws after commas)

---

_Pull request opened by @akx on 2023-07-26 12:55_

## Summary

This PR implements pycodestyle's E241 (tab after comma) and E242 (multiple whitespace after comma) lints.

These are marked as nursery rules like many other pycodestyle rules.

Refs #2402

## Test Plan

E24.py copied from pycodestyle.

---

_Comment by @github-actions[bot] on 2023-07-26 13:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.3±0.09ms     3.9 MB/sec    1.00     10.2±0.08ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1974.9±11.61µs     8.4 MB/sec    1.00  1973.2±21.28µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    214.6±2.60µs    13.8 MB/sec    1.00    215.5±1.27µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.04ms     6.0 MB/sec    1.00      4.3±0.03ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.06     14.4±1.23ms     2.8 MB/sec    1.00     13.6±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.02ms     4.8 MB/sec    1.00      3.4±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    463.1±6.24µs     6.4 MB/sec    1.00    458.2±2.01µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.07ms     4.0 MB/sec    1.00      6.2±0.15ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.05      7.5±0.06ms     5.4 MB/sec    1.00      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04   1532.4±4.98µs    10.9 MB/sec    1.00  1478.1±13.76µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.05    166.8±0.48µs    17.7 MB/sec    1.00    158.8±0.96µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.3±0.04ms     7.8 MB/sec    1.00      3.1±0.02ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.8±0.63ms     2.9 MB/sec    1.03     14.2±0.53ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.18ms     6.6 MB/sec    1.03      2.6±0.13ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   276.7±14.82µs    10.7 MB/sec    1.04   287.4±16.88µs    10.3 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.26ms     4.5 MB/sec    1.02      5.8±0.23ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.02     17.1±0.46ms     2.4 MB/sec    1.00     16.7±0.49ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.4±0.22ms     3.8 MB/sec    1.00      4.3±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   536.2±18.92µs     5.5 MB/sec    1.00   536.1±24.20µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.5±0.25ms     3.4 MB/sec    1.00      7.4±0.28ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.03      9.5±0.63ms     4.3 MB/sec    1.00      9.2±0.35ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1899.2±70.17µs     8.8 MB/sec    1.00  1903.1±70.90µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   226.3±10.94µs    13.0 MB/sec    1.00    220.9±9.78µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.19ms     6.2 MB/sec    1.00      4.0±0.17ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-27 02:36_

---

_@MichaReiser approved on 2023-07-27 06:14_

---

_Review comment by @konstin on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/space_around_operator.rs`:150 on 2023-07-27 11:39_

```suggestion
/// Checks for extraneous whitespace after a comma.
```

---

_Review comment by @konstin on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/space_around_operator.rs`:157 on 2023-07-27 11:40_

nit: makes the example a little easier to stop
```suggestion
/// a = 4,      5
```

---

_@konstin approved on 2023-07-27 11:41_

---

_Comment by @konstin on 2023-07-27 11:57_

(i added the new docstrings to the docs black formatting ignore list)

---

_Label `rule` added by @charliermarsh on 2023-07-27 18:47_

---

_Merged by @charliermarsh on 2023-07-27 18:58_

---

_Closed by @charliermarsh on 2023-07-27 18:58_

---
