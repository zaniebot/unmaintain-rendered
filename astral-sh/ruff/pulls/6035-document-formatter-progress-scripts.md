```yaml
number: 6035
title: Document formatter progress scripts
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: document_formatter_progress_scripts
created_at: 2023-07-24T15:37:18Z
updated_at: 2023-07-24T17:42:21Z
url: https://github.com/astral-sh/ruff/pull/6035
synced_at: 2026-01-12T03:30:22Z
```

# Document formatter progress scripts

---

_Pull request opened by @konstin on 2023-07-24 15:37_

## Summary

Add documentation to the formatter progress scripts

## Test Plan

n/a


---

_Label `internal` added by @konstin on 2023-07-24 15:37_

---

_Label `formatter` added by @konstin on 2023-07-24 15:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:239 on 2023-07-24 15:52_

Is there a good resource/reference for users to know whether the numbers are improving or regressing compared to main? I looked at the result at my other PR and wasn't sure against what I should compare the numbers.

---

_@MichaReiser approved on 2023-07-24 15:52_

---

_Comment by @github-actions[bot] on 2023-07-24 16:12_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.03ms     4.2 MB/sec    1.00      9.7±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1890.3±4.54µs     8.8 MB/sec    1.00   1897.2±4.29µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    206.4±2.18µs    14.3 MB/sec    1.01    208.0±2.39µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.01      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.04ms     3.0 MB/sec    1.00     13.4±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.0±0.87µs     8.1 MB/sec    1.00    363.3±1.18µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.04ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1430.0±2.57µs    11.6 MB/sec    1.01   1439.3±4.68µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.1±0.45µs    19.5 MB/sec    1.00    151.0±0.41µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.5±0.12ms     3.9 MB/sec    1.00     10.4±0.13ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.2 MB/sec    1.01      2.0±0.08ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    229.3±7.93µs    12.9 MB/sec    1.04   237.3±14.98µs    12.4 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.08ms     5.7 MB/sec    1.01      4.5±0.08ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.7±0.18ms     2.8 MB/sec    1.00     14.8±0.20ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.10ms     4.2 MB/sec    1.00      3.9±0.08ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   462.0±14.30µs     6.4 MB/sec    1.01   466.7±31.23µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.11ms     3.7 MB/sec    1.00      6.8±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.08ms     5.3 MB/sec    1.01      7.8±0.08ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1584.4±26.54µs    10.5 MB/sec    1.01  1598.1±27.13µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.8±3.92µs    16.5 MB/sec    1.01    180.4±5.19µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.05ms     7.5 MB/sec    1.00      3.4±0.05ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin reviewed on 2023-07-24 17:42_

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:239 on 2023-07-24 17:42_

not really, but i'd rather solve the tracking-numbers-from-CI in general over building more CI-interacting script

---

_Merged by @konstin on 2023-07-24 17:42_

---

_Closed by @konstin on 2023-07-24 17:42_

---

_Branch deleted on 2023-07-24 17:42_

---
