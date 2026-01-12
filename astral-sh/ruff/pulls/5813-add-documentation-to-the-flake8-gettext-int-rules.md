```yaml
number: 5813
title: "Add documentation to the `flake8-gettext` (`INT`) rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: gettext-docs
created_at: 2023-07-16T21:43:18Z
updated_at: 2023-07-17T09:39:17Z
url: https://github.com/astral-sh/ruff/pull/5813
synced_at: 2026-01-12T15:55:19Z
```

# Add documentation to the `flake8-gettext` (`INT`) rules

---

_@tjkuson_

## Summary

Completes documentation for the `flake8-gettext` (`INT`) ruleset. Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-07-16 21:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.4±0.29ms     3.9 MB/sec    1.00     10.3±0.29ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.08ms     8.0 MB/sec    1.02      2.1±0.07ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    229.0±9.02µs    12.9 MB/sec    1.02    234.4±8.71µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.12ms     5.8 MB/sec    1.00      4.4±0.14ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.5±0.39ms     2.8 MB/sec    1.01     14.7±0.41ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.11ms     4.5 MB/sec    1.00      3.7±0.09ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   486.9±17.82µs     6.1 MB/sec    1.00   486.9±16.22µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.20ms     3.9 MB/sec    1.00      6.5±0.21ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.18ms     5.7 MB/sec    1.02      7.3±0.23ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1571.6±49.62µs    10.6 MB/sec    1.00  1566.2±49.76µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.6±4.93µs    16.7 MB/sec    1.00    175.6±6.36µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.09ms     7.9 MB/sec    1.01      3.3±0.10ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.6±0.37ms     3.0 MB/sec    1.00     13.6±0.40ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.10ms     6.2 MB/sec    1.01      2.7±0.12ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   304.9±16.26µs     9.7 MB/sec    1.05   321.3±31.95µs     9.2 MB/sec
formatter/pydantic/types.py                1.00      5.8±0.21ms     4.4 MB/sec    1.01      5.9±0.20ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.00     19.3±0.40ms     2.1 MB/sec    1.00     19.3±0.50ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.29ms     3.3 MB/sec    1.00      5.1±0.17ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   612.0±22.06µs     4.8 MB/sec    1.00   611.6±20.82µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.26ms     2.9 MB/sec    1.00      8.7±0.19ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.27ms     4.1 MB/sec    1.00      9.9±0.37ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.07ms     7.9 MB/sec    1.00      2.1±0.06ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    240.3±8.59µs    12.3 MB/sec    1.00   241.1±10.77µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.14ms     5.8 MB/sec    1.01      4.4±0.14ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-17 04:03_

---

_Merged by @charliermarsh on 2023-07-17 04:09_

---

_Closed by @charliermarsh on 2023-07-17 04:09_

---

_Branch deleted on 2023-07-17 09:39_

---
