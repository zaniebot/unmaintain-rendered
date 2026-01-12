```yaml
number: 5317
title: "Fix `diagnostics` variable name in `add_plugin.py` script"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: fix/add-plugin-script-diagnostics-variable
created_at: 2023-06-22T19:54:45Z
updated_at: 2023-06-22T20:28:27Z
url: https://github.com/astral-sh/ruff/pull/5317
synced_at: 2026-01-12T03:36:54Z
```

# Fix `diagnostics` variable name in `add_plugin.py` script

---

_Pull request opened by @edgarrmondragon on 2023-06-22 19:54_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix a variable name in the `add_plugin.py` script.

## Test Plan

<!-- How was it tested? -->

I don't think there are any tests for the scripts, other than manual confirmation

---

_Comment by @charliermarsh on 2023-06-22 20:00_

Thanks!

---

_Merged by @charliermarsh on 2023-06-22 20:06_

---

_Closed by @charliermarsh on 2023-06-22 20:06_

---

_Comment by @github-actions[bot] on 2023-06-22 20:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.07ms     5.3 MB/sec    1.00      7.8±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1699.3±10.96µs     9.8 MB/sec    1.00  1703.2±17.87µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    189.3±0.79µs    15.6 MB/sec    1.00    189.6±1.02µs    15.6 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.03ms     6.5 MB/sec    1.00      3.9±0.05ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.16ms     2.7 MB/sec    1.01     15.5±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.03ms     4.3 MB/sec    1.01      3.9±0.03ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.3±2.29µs     6.0 MB/sec    1.00    496.4±3.69µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.00      6.8±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.05ms     5.2 MB/sec    1.00      7.8±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1697.7±17.91µs     9.8 MB/sec    1.02  1724.2±14.95µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.4±1.02µs    15.5 MB/sec    1.02    193.3±1.35µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.2 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      9.8±0.40ms     4.1 MB/sec    1.00      9.4±0.53ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.1±0.13ms     7.8 MB/sec    1.00  1985.1±107.51µs     8.4 MB/sec
formatter/numpy/globals.py                 1.04   242.6±15.17µs    12.2 MB/sec    1.00   233.0±19.84µs    12.7 MB/sec
formatter/pydantic/types.py                1.05      4.8±0.26ms     5.3 MB/sec    1.00      4.6±0.28ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.09     19.7±1.01ms     2.1 MB/sec    1.00     18.1±1.03ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.27ms     3.4 MB/sec    1.01      4.9±0.20ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   562.5±30.78µs     5.2 MB/sec    1.20   674.0±36.14µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.50ms     3.2 MB/sec    1.07      8.6±0.51ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.51ms     4.3 MB/sec    1.11     10.4±0.42ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.11ms     8.1 MB/sec    1.02      2.1±0.13ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.07   250.9±25.59µs    11.8 MB/sec    1.00   233.7±10.62µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.3±0.21ms     5.9 MB/sec    1.00      4.1±0.25ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-06-22 20:10_

---
