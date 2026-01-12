```yaml
number: 5025
title: "Allow `Options`-to-`Settings` conversion to use `TryFrom`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/result-option
created_at: 2023-06-12T15:25:28Z
updated_at: 2023-06-12T15:51:05Z
url: https://github.com/astral-sh/ruff/pull/5025
synced_at: 2026-01-12T15:55:17Z
```

# Allow `Options`-to-`Settings` conversion to use `TryFrom`

---

_@charliermarsh_

## Summary

This avoids a bad `expect()` call in the `copyright` conversion.

## Test Plan

`cargo test`

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/mod.rs`:240 on 2023-06-12 15:26_

These are mostly no-op changes to use the `from` target instead of `Into::into`. We _have_ to do that for any of the options that implement `TryFrom` instead of `From`, so I prefer to be consistent.

---

_@charliermarsh reviewed on 2023-06-12 15:26_

---

_Merged by @charliermarsh on 2023-06-12 15:31_

---

_Closed by @charliermarsh on 2023-06-12 15:31_

---

_Branch deleted on 2023-06-12 15:31_

---

_Comment by @github-actions[bot] on 2023-06-12 15:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.04ms     5.3 MB/sec    1.00      7.7±0.04ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1602.0±4.43µs    10.4 MB/sec    1.00  1605.7±10.00µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    154.7±0.29µs    19.1 MB/sec    1.01    155.8±1.29µs    18.9 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.06ms     2.4 MB/sec    1.00     16.9±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    507.4±1.00µs     5.8 MB/sec    1.00    507.6±4.17µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.05ms     3.5 MB/sec    1.00      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.03ms     5.0 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1756.8±3.36µs     9.5 MB/sec    1.00  1755.6±10.48µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.4±0.38µs    15.1 MB/sec    1.00    195.5±1.22µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.02ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.1±0.40ms     4.0 MB/sec    1.00      9.9±0.44ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.15ms     8.1 MB/sec    1.00      2.0±0.12ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   197.6±12.40µs    14.9 MB/sec    1.00   197.3±16.81µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.19ms     6.3 MB/sec    1.04      4.2±0.25ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.02     21.5±0.55ms  1937.2 KB/sec    1.00     21.1±0.79ms  1971.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.5±0.22ms     3.0 MB/sec    1.00      5.2±0.25ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   643.8±30.39µs     4.6 MB/sec    1.00   622.3±26.50µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.06      9.3±0.39ms     2.7 MB/sec    1.00      8.8±0.33ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.04     10.8±0.35ms     3.8 MB/sec    1.00     10.4±0.37ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.3±0.10ms     7.3 MB/sec    1.00      2.2±0.10ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   254.7±13.75µs    11.6 MB/sec    1.02   260.2±21.46µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.9±0.26ms     5.2 MB/sec    1.00      4.7±0.22ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
