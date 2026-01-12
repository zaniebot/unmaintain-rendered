```yaml
number: 6115
title: "Add `PT017` and `PT019` docs"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT017-PT019-docs
created_at: 2023-07-27T03:09:24Z
updated_at: 2023-07-27T19:16:27Z
url: https://github.com/astral-sh/ruff/pull/6115
synced_at: 2026-01-12T03:30:22Z
```

# Add `PT017` and `PT019` docs

---

_Pull request opened by @harupy on 2023-07-27 03:09_

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

Existing checks

---

_Comment by @github-actions[bot] on 2023-07-27 03:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.49ms     3.7 MB/sec    1.02     11.3±0.35ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.14ms     7.7 MB/sec    1.00      2.2±0.07ms     7.7 MB/sec
formatter/numpy/globals.py                 1.01   238.7±10.81µs    12.4 MB/sec    1.00   236.3±12.07µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.22ms     5.4 MB/sec    1.01      4.7±0.19ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     16.0±0.50ms     2.5 MB/sec    1.00     15.8±0.54ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.23ms     4.4 MB/sec    1.05      4.0±0.25ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   470.1±38.37µs     6.3 MB/sec    1.09   514.6±22.69µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.35ms     3.7 MB/sec    1.07      7.5±0.28ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.69ms     5.3 MB/sec    1.11      8.5±0.40ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1603.3±65.78µs    10.4 MB/sec    1.03  1645.1±49.54µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   174.7±13.44µs    16.9 MB/sec    1.09    190.6±7.06µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.32ms     7.5 MB/sec    1.05      3.6±0.12ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.0±0.73ms     3.1 MB/sec    1.23     15.9±0.64ms     2.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.13ms     7.1 MB/sec    1.17      2.7±0.19ms     6.1 MB/sec
formatter/numpy/globals.py                 1.00   260.4±16.91µs    11.3 MB/sec    1.20   313.6±18.20µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.21ms     4.6 MB/sec    1.12      6.2±0.34ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.03     18.7±0.74ms     2.2 MB/sec    1.00     18.2±0.69ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.8±0.28ms     3.4 MB/sec    1.00      4.7±0.25ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.04   577.8±18.72µs     5.1 MB/sec    1.00   553.2±33.50µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.08      8.5±0.30ms     3.0 MB/sec    1.00      7.8±0.40ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.06     10.7±0.41ms     3.8 MB/sec    1.00     10.1±0.80ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.07ms     8.2 MB/sec    1.00      2.0±0.14ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.03   241.2±16.25µs    12.2 MB/sec    1.00   233.9±11.86µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.08      4.5±0.23ms     5.7 MB/sec    1.00      4.2±0.15ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @MichaReiser on 2023-07-27 05:57_

---

_Merged by @charliermarsh on 2023-07-27 18:56_

---

_Closed by @charliermarsh on 2023-07-27 18:56_

---
