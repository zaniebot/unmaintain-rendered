```yaml
number: 5517
title: "[`pylint`] Implement Pylint `typevar-double-variance` (`C0131`) "
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: bivariance
created_at: 2023-07-04T23:18:24Z
updated_at: 2023-07-10T09:54:54Z
url: https://github.com/astral-sh/ruff/pull/5517
synced_at: 2026-01-12T15:55:18Z
```

# [`pylint`] Implement Pylint `typevar-double-variance` (`C0131`) 

---

_@tjkuson_

## Summary

Implement Pylint `typevar-double-variance` (`C0131`)  as `type-bivariance` (`PLC0131`). Includes documentation. Related to #970. Renamed the rule to be more clear (it's not immediately obvious what 'double' means, IMO).

The Pylint implementation checks only `TypeVar`, but this PR checks `ParamSpec` as well.

## Test Plan

Added tests.

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-04 23:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.13     11.1±0.42ms     3.7 MB/sec     1.00      9.9±0.42ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.11ms     7.2 MB/sec     1.00      2.3±0.19ms     7.2 MB/sec
formatter/numpy/globals.py                 1.17   287.7±24.25µs    10.3 MB/sec     1.00   246.5±11.51µs    12.0 MB/sec
formatter/pydantic/types.py                1.06      5.2±0.27ms     4.9 MB/sec     1.00      4.9±0.25ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     17.8±1.10ms     2.3 MB/sec     1.05     18.8±0.79ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.25ms     3.9 MB/sec     1.04      4.5±0.20ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   553.2±25.56µs     5.3 MB/sec     1.03   571.5±28.75µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.56ms     3.2 MB/sec     1.00      7.9±0.42ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.43ms     4.7 MB/sec     1.09      9.5±0.35ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1879.6±111.86µs     8.9 MB/sec    1.06  1987.1±82.65µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.5±10.71µs    13.6 MB/sec     1.08   234.6±16.92µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.25ms     6.4 MB/sec     1.06      4.2±0.22ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.0±0.49ms     4.1 MB/sec    1.00      9.7±0.30ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.15ms     7.6 MB/sec    1.00      2.1±0.07ms     7.9 MB/sec
formatter/numpy/globals.py                 1.01   244.8±12.41µs    12.1 MB/sec    1.00   241.6±22.83µs    12.2 MB/sec
formatter/pydantic/types.py                1.02      4.8±0.22ms     5.3 MB/sec    1.00      4.7±0.18ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.04     17.6±0.58ms     2.3 MB/sec    1.00     16.8±0.51ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.5±0.24ms     3.7 MB/sec    1.00      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04   546.4±43.80µs     5.4 MB/sec    1.00   526.9±22.24µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.8±0.36ms     3.3 MB/sec    1.00      7.5±0.29ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.04      8.8±0.39ms     4.6 MB/sec    1.00      8.4±0.22ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1864.8±77.93µs     8.9 MB/sec    1.00  1798.5±72.74µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.05   220.2±13.75µs    13.4 MB/sec    1.00    209.4±8.45µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.9±0.14ms     6.5 MB/sec    1.00      3.8±0.16ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-05 04:11_

---

_Merged by @charliermarsh on 2023-07-05 18:53_

---

_Closed by @charliermarsh on 2023-07-05 18:53_

---

_Branch deleted on 2023-07-10 09:54_

---
