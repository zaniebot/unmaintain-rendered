```yaml
number: 6608
title: "Format `PatternMatchValue`"
type: pull_request
state: closed
author: evanrittenhouse
labels:
  - formatter
assignees: []
draft: true
base: main
head: evanrittenhouse/6555
created_at: 2023-08-16T03:53:59Z
updated_at: 2023-08-23T06:32:09Z
url: https://github.com/astral-sh/ruff/pull/6608
synced_at: 2026-01-12T02:45:38Z
```

# Format `PatternMatchValue`

---

_Pull request opened by @evanrittenhouse on 2023-08-16 03:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-16 04:23_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.23ms    10.1 MB/sec    1.04      4.2±0.16ms     9.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   815.7±37.53µs    20.4 MB/sec    1.07   870.0±19.24µs    19.1 MB/sec
formatter/numpy/globals.py                 1.00     80.9±4.65µs    36.5 MB/sec    1.08     87.4±3.42µs    33.8 MB/sec
formatter/pydantic/types.py                1.00  1562.0±63.05µs    16.3 MB/sec    1.08  1684.9±80.34µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.36ms     3.3 MB/sec    1.06     13.2±0.46ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.06ms     5.1 MB/sec    1.05      3.4±0.10ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   473.9±10.18µs     6.2 MB/sec    1.05   498.0±16.29µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.19ms     4.0 MB/sec    1.05      6.7±0.19ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.18ms     6.3 MB/sec    1.08      7.0±0.17ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1489.6±79.59µs    11.2 MB/sec    1.02  1521.4±40.34µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.4±7.09µs    16.5 MB/sec    1.04    185.4±5.94µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.06ms     8.9 MB/sec    1.08      3.1±0.07ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.8±0.27ms     8.6 MB/sec    1.00      4.7±0.21ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  963.0±145.09µs    17.3 MB/sec    1.01   970.4±46.00µs    17.2 MB/sec
formatter/numpy/globals.py                 1.00     95.6±5.79µs    30.9 MB/sec    1.00     95.8±6.19µs    30.8 MB/sec
formatter/pydantic/types.py                1.00  1865.3±89.05µs    13.7 MB/sec    1.01  1892.5±74.19µs    13.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.61ms     2.6 MB/sec    1.02     15.9±0.52ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.10ms     3.8 MB/sec    1.00      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   546.1±26.14µs     5.4 MB/sec    1.01   549.8±21.83µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.35ms     3.1 MB/sec    1.00      8.2±0.27ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      8.7±0.32ms     4.7 MB/sec    1.00      8.6±0.22ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1820.9±75.76µs     9.1 MB/sec    1.00  1822.9±67.13µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    230.4±8.30µs    12.8 MB/sec    1.00    226.9±8.20µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.12ms     6.6 MB/sec    1.00      3.8±0.15ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @konstin on 2023-08-21 08:32_

---

_Comment by @konstin on 2023-08-21 17:43_

> [Compiler panic?](https://github.com/astral-sh/ruff/pull/6608/commits/cbc2e272adb820261cf812b7c658988e98d66cfe)

Did you run into https://github.com/rust-lang/rust/issues/111513?

---

_Comment by @charliermarsh on 2023-08-23 02:00_

I renewed this as https://github.com/astral-sh/ruff/pull/6799 since I'd like to keep working on some related problems around comments and parentheses.

---

_Closed by @MichaReiser on 2023-08-23 06:32_

---
