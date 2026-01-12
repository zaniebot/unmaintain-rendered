```yaml
number: 6521
title: Add PT002 ~ PT005 docs
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT002345
created_at: 2023-08-12T05:51:45Z
updated_at: 2023-08-14T22:27:57Z
url: https://github.com/astral-sh/ruff/pull/6521
synced_at: 2026-01-12T15:55:21Z
```

# Add PT002 ~ PT005 docs

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

#2646 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-12 06:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02      5.0±0.33ms     8.2 MB/sec     1.00      4.9±0.27ms     8.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   954.5±43.52µs    17.4 MB/sec     1.01   965.9±47.76µs    17.2 MB/sec
formatter/numpy/globals.py                 1.07     97.5±9.09µs    30.3 MB/sec     1.00     91.3±5.44µs    32.3 MB/sec
formatter/pydantic/types.py                1.00  1904.3±112.35µs    13.4 MB/sec    1.01  1922.3±108.67µs    13.3 MB/sec
linter/all-rules/large/dataset.py          1.01     14.7±0.34ms     2.8 MB/sec     1.00     14.6±0.45ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.11ms     4.4 MB/sec     1.00      3.8±0.17ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.2±22.54µs     5.6 MB/sec     1.00   533.6±19.97µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.6±0.27ms     3.3 MB/sec     1.00      7.4±0.26ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      7.8±0.24ms     5.2 MB/sec     1.00      7.7±0.23ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1665.4±62.72µs    10.0 MB/sec     1.00  1599.8±54.72µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.04   204.9±20.87µs    14.4 MB/sec     1.00    196.4±8.46µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.15ms     7.4 MB/sec     1.00      3.3±0.13ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.2±0.07ms     9.6 MB/sec    1.00      4.2±0.06ms     9.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   823.4±11.33µs    20.2 MB/sec    1.00   820.9±13.34µs    20.3 MB/sec
formatter/numpy/globals.py                 1.00     84.1±2.53µs    35.1 MB/sec    1.02     85.8±4.76µs    34.4 MB/sec
formatter/pydantic/types.py                1.00  1682.7±30.78µs    15.2 MB/sec    1.01  1692.7±45.34µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.27ms     3.2 MB/sec    1.02     13.1±0.28ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.8 MB/sec    1.01      3.5±0.05ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.1±6.54µs     7.0 MB/sec    1.03    436.0±9.02µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.14ms     3.9 MB/sec    1.05      6.9±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.09ms     5.9 MB/sec    1.01      7.0±0.09ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1444.0±23.41µs    11.5 MB/sec    1.02  1471.4±17.22µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±2.26µs    17.9 MB/sec    1.04    170.8±2.44µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.07ms     8.4 MB/sec    1.02      3.1±0.04ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-14 21:18_

---

_Merged by @charliermarsh on 2023-08-14 21:29_

---

_Closed by @charliermarsh on 2023-08-14 21:29_

---

_Branch deleted on 2023-08-14 22:27_

---
