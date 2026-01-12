```yaml
number: 5457
title: "[`flake8-pyi`] Implement PYI002, PYI003, PYI004, PYI005"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI-comparisons
created_at: 2023-07-01T19:55:07Z
updated_at: 2023-07-03T13:07:55Z
url: https://github.com/astral-sh/ruff/pull/5457
synced_at: 2026-01-12T15:55:18Z
```

# [`flake8-pyi`] Implement PYI002, PYI003, PYI004, PYI005

---

_@density_

## Summary

Implements flake8-pyi checks 002, 003, 004, 005. The logic is a bit complex, as you can see in the [original code](https://github.com/PyCQA/flake8-pyi/blob/57921813c1fb5b92f810a57753850c212fcc29b0/pyi.py#L1403C18-L1403C18).

ref: #848 

## Test Plan

Updated snapshot tests. Ran flake8 to double check lints, and ran ruff with all PYI lints enabled to check for incorrect overlapping lint errors.


---

_Comment by @density on 2023-07-01 20:00_

Tests are failing with `Rule index out of bounds. Increase the size of the bitset array.`, which I assume means we've added enough rules (yay) that it's time to increase the size? I defer to the maintainers on what the best course of action is.

---

_Marked ready for review by @density on 2023-07-01 20:00_

---

_Comment by @charliermarsh on 2023-07-01 21:03_

Do you mind bumping `const RULESET_SIZE: usize = 10;` to `const RULESET_SIZE: usize = 11;` in `registry/rule_set.rs` as part of this PR?

---

_Comment by @density on 2023-07-01 21:29_

Done @charliermarsh 

---

_Comment by @github-actions[bot] on 2023-07-01 21:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01     10.3±0.32ms     4.0 MB/sec     1.00     10.2±0.47ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.09ms     7.5 MB/sec     1.00      2.2±0.09ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   254.8±14.29µs    11.6 MB/sec     1.10   281.6±44.16µs    10.5 MB/sec
formatter/pydantic/types.py                1.01      4.9±0.16ms     5.2 MB/sec     1.00      4.8±0.19ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     18.5±0.68ms     2.2 MB/sec     1.00     18.1±0.66ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.16ms     3.8 MB/sec     1.01      4.4±0.22ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   564.8±24.07µs     5.2 MB/sec     1.00   565.8±22.85µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.9±0.34ms     3.2 MB/sec     1.00      7.7±0.26ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.38ms     4.5 MB/sec     1.00      8.8±0.26ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1963.9±128.99µs     8.5 MB/sec    1.00  1838.4±73.07µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.13   251.6±21.25µs    11.7 MB/sec     1.00   222.5±13.02µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.1±0.17ms     6.2 MB/sec     1.00      3.9±0.14ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     12.6±0.32ms     3.2 MB/sec    1.00     12.0±1.05ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.10ms     6.3 MB/sec    1.02      2.7±0.28ms     6.1 MB/sec
formatter/numpy/globals.py                 1.01    285.8±8.87µs    10.3 MB/sec    1.00   281.7±33.92µs    10.5 MB/sec
formatter/pydantic/types.py                1.03      5.8±0.22ms     4.4 MB/sec    1.00      5.7±0.49ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.02     20.2±0.47ms     2.0 MB/sec    1.00     19.8±0.29ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.3±0.18ms     3.1 MB/sec    1.00      5.2±0.14ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   630.0±27.20µs     4.7 MB/sec    1.00   626.3±25.79µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.1±0.29ms     2.8 MB/sec    1.00      8.8±0.23ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.2±0.25ms     4.0 MB/sec    1.00     10.0±0.21ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.07ms     7.7 MB/sec    1.00      2.2±0.07ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    248.5±8.16µs    11.9 MB/sec    1.03   255.7±34.82µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.6±0.15ms     5.6 MB/sec    1.00      4.5±0.13ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-03 03:41_

---

_Merged by @charliermarsh on 2023-07-03 03:52_

---

_Closed by @charliermarsh on 2023-07-03 03:52_

---

_Branch deleted on 2023-07-03 13:07_

---
