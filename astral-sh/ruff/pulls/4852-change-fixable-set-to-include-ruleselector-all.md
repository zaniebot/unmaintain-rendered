```yaml
number: 4852
title: "Change fixable_set to include RuleSelector::All/Nursery"
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 4761_fix_nursery_rules
created_at: 2023-06-04T16:07:07Z
updated_at: 2023-06-15T21:15:27Z
url: https://github.com/astral-sh/ruff/pull/4852
synced_at: 2026-01-12T15:55:16Z
```

# Change fixable_set to include RuleSelector::All/Nursery

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #4761. 

This PR merges `RuleSelector::All` and `RuleSelector::Nursery` by default into `Configuration`'s `fixable_set`.

## Test Plan

`cargo run -p ruff_cli -- crates/ruff/resources/test/fixtures/pycodestyle/E20.py --no-cache --select E202,E203,E201 --fix`

On `main`, this command will detect 19 fixes, but won't fix any of them. On my branch, this command will fix all 19 errors.


---

_Comment by @github-actions[bot] on 2023-06-04 16:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.9±0.42ms     2.3 MB/sec    1.00     17.6±0.47ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.17ms     3.9 MB/sec    1.00      4.2±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.3±15.59µs     5.6 MB/sec    1.01   534.5±21.28µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.18ms     3.4 MB/sec    1.00      7.4±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.05      8.9±0.33ms     4.6 MB/sec    1.00      8.5±0.20ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1901.0±66.82µs     8.8 MB/sec    1.00  1868.4±85.21µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   222.3±10.27µs    13.3 MB/sec    1.00    222.7±8.63µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.0±0.13ms     6.4 MB/sec    1.00      3.9±0.13ms     6.5 MB/sec
parser/large/dataset.py                    1.02      6.7±0.24ms     6.0 MB/sec    1.00      6.6±0.16ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.02  1334.9±47.60µs    12.5 MB/sec    1.00  1309.8±49.12µs    12.7 MB/sec
parser/numpy/globals.py                    1.01    131.4±3.97µs    22.4 MB/sec    1.00    130.6±4.93µs    22.6 MB/sec
parser/pydantic/types.py                   1.04      3.0±0.17ms     8.5 MB/sec    1.00      2.9±0.12ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.15ms     2.3 MB/sec    1.00     17.4±0.10ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.04ms     3.8 MB/sec    1.02      4.5±0.05ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.2±9.87µs     6.6 MB/sec    1.00    446.2±6.57µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.08ms     3.5 MB/sec    1.00      7.4±0.09ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.7±0.13ms     4.7 MB/sec    1.00      8.6±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1825.5±12.65µs     9.1 MB/sec    1.00  1817.9±18.36µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.2±1.58µs    15.0 MB/sec    1.01    198.3±6.93µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.02ms     6.5 MB/sec    1.00      3.9±0.07ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.7±0.06ms     6.1 MB/sec    1.01      6.7±0.03ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1259.5±10.35µs    13.2 MB/sec    1.02   1280.3±9.72µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    131.1±1.30µs    22.5 MB/sec    1.01    132.4±2.20µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.02      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-05 02:25_

---

_Closed by @charliermarsh on 2023-06-05 02:25_

---

_Branch deleted on 2023-06-15 21:15_

---
