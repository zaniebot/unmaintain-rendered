```yaml
number: 6532
title: "Remove `import pytest` in `flake8-pytest-style` docs for consistency"
type: pull_request
state: closed
author: harupy
labels: []
assignees: []
base: main
head: remove-import-pytest
created_at: 2023-08-13T06:46:56Z
updated_at: 2023-08-14T03:11:38Z
url: https://github.com/astral-sh/ruff/pull/6532
synced_at: 2026-01-12T15:55:21Z
```

# Remove `import pytest` in `flake8-pytest-style` docs for consistency

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

_Comment by @github-actions[bot] on 2023-08-13 06:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.6±0.35ms     3.9 MB/sec    1.00     10.1±0.36ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.1±0.18ms     8.1 MB/sec    1.00  1978.6±62.33µs     8.4 MB/sec
formatter/numpy/globals.py                 1.04   238.7±21.25µs    12.4 MB/sec    1.00   229.5±16.02µs    12.9 MB/sec
formatter/pydantic/types.py                1.04      4.4±0.24ms     5.8 MB/sec    1.00      4.2±0.18ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.36ms     3.0 MB/sec    1.00     13.6±0.42ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.11ms     4.7 MB/sec    1.01      3.6±0.14ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   527.1±28.50µs     5.6 MB/sec    1.01   531.6±23.25µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.24ms     3.6 MB/sec    1.03      7.2±0.23ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.03      7.4±0.22ms     5.5 MB/sec    1.00      7.2±0.24ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1563.5±54.80µs    10.7 MB/sec    1.00  1556.9±63.21µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    194.9±9.04µs    15.1 MB/sec    1.00    190.2±8.77µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.09ms     7.8 MB/sec    1.00      3.3±0.26ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.23ms     4.0 MB/sec    1.00     10.0±0.09ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.02  1994.2±84.05µs     8.3 MB/sec    1.00  1964.4±47.21µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    214.2±3.79µs    13.8 MB/sec    1.03   220.3±14.61µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.09ms     6.0 MB/sec    1.00      4.2±0.07ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.25ms     3.2 MB/sec    1.00     12.8±0.21ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.09ms     4.7 MB/sec    1.00      3.5±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   453.3±47.20µs     6.5 MB/sec    1.00   443.8±11.05µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.15ms     3.8 MB/sec    1.00      6.7±0.20ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.23ms     5.8 MB/sec    1.01      7.0±0.13ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1480.1±24.22µs    11.3 MB/sec    1.00  1483.5±26.08µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.8±7.62µs    16.6 MB/sec    1.00    177.6±6.14µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.08ms     8.2 MB/sec    1.00      3.1±0.07ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-08-13 14:46_

I think we've trended towards having the examples be executable (i.e. they include the necessary imports). Is there a reason you went with removal to these instead of addition to the others?

---

_Comment by @harupy on 2023-08-14 00:18_

perhaps I should do the opposite (adding `import pytest`).

---

_Comment by @charliermarsh on 2023-08-14 03:10_

Yeah I'd probably vote to add to the other examples rather than remove.

---

_Comment by @harupy on 2023-08-14 03:11_

Sounds good :) I'll close this PR and file a new one.

---

_Closed by @harupy on 2023-08-14 03:11_

---
