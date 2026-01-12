```yaml
number: 5786
title: Include alias when formatting import-from structs
type: pull_request
state: merged
author: guillaumeLepape
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: I002_fix
created_at: 2023-07-15T19:41:56Z
updated_at: 2023-07-15T20:12:23Z
url: https://github.com/astral-sh/ruff/pull/5786
synced_at: 2026-01-12T03:30:21Z
```

# Include alias when formatting import-from structs

---

_Pull request opened by @guillaumeLepape on 2023-07-15 19:41_

When required-imports is set with the syntax from ... import ... as ..., autofix I002 is failing

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Reuse the same python files as `crates/ruff/src/rules/isort/mod.rs:required_import` test.

<!-- How was it tested? -->


---

_Renamed from "I002 fix" to "Include alias when formatting import-from structs" by @charliermarsh on 2023-07-15 19:50_

---

_Label `bug` added by @charliermarsh on 2023-07-15 19:50_

---

_Label `isort` added by @charliermarsh on 2023-07-15 19:50_

---

_Comment by @charliermarsh on 2023-07-15 19:50_

Thanks -- looks right to me, must've been an oversight.

---

_Merged by @charliermarsh on 2023-07-15 19:53_

---

_Closed by @charliermarsh on 2023-07-15 19:53_

---

_Comment by @github-actions[bot] on 2023-07-15 19:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.03ms     4.1 MB/sec    1.00      9.8±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1903.9±2.60µs     8.7 MB/sec    1.00   1887.2±5.04µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    206.3±0.34µs    14.3 MB/sec    1.00    205.5±2.27µs    14.4 MB/sec
formatter/pydantic/types.py                1.03      4.3±0.07ms     6.0 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.07ms     2.9 MB/sec    1.01     14.1±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    382.6±4.04µs     7.7 MB/sec    1.00    383.1±0.88µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.01      6.3±0.10ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.01      7.1±0.04ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1452.2±3.16µs    11.5 MB/sec    1.01   1465.4±5.44µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.9±0.47µs    19.1 MB/sec    1.01    157.0±0.59µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.01      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.13ms     3.7 MB/sec    1.12     12.4±0.16ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.09      2.4±0.02ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00    244.7±4.86µs    12.1 MB/sec    1.10   268.2±35.17µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.11      5.2±0.07ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.03     16.2±0.18ms     2.5 MB/sec    1.00     15.8±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.07ms     3.9 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   516.6±20.18µs     5.7 MB/sec    1.00    505.4±7.87µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.09ms     3.5 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.07ms     5.0 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1731.5±16.22µs     9.6 MB/sec    1.00  1709.7±16.44µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.4±3.48µs    14.6 MB/sec    1.00    201.4±5.50µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.07ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-07-15 19:53_

---
