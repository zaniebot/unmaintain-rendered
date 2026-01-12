```yaml
number: 5879
title: "Add known problems to `compare-to-empty-string` documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: compare-to-str-known-problems
created_at: 2023-07-19T10:34:19Z
updated_at: 2023-07-20T11:10:17Z
url: https://github.com/astral-sh/ruff/pull/5879
synced_at: 2026-01-12T15:55:19Z
```

# Add known problems to `compare-to-empty-string` documentation

---

_@tjkuson_

## Summary

Add known problems to `compare-to-empty-string` documentation. Related to #5873.

Tweaked the example in the documentation to be a tad more concise and correct (that the rule is most applicable when comparing to a `str` variable).

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-07-19 10:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     10.5±0.03ms     3.9 MB/sec    1.00      9.8±0.02ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.0±0.02ms     8.3 MB/sec    1.00   1891.4±5.72µs     8.8 MB/sec
formatter/numpy/globals.py                 1.03    215.0±1.36µs    13.7 MB/sec    1.00    207.9±0.71µs    14.2 MB/sec
formatter/pydantic/types.py                1.06      4.5±0.09ms     5.7 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.10ms     3.0 MB/sec    1.00     13.7±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.2±4.28µs     7.9 MB/sec    1.00    375.6±4.41µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1438.7±4.48µs    11.6 MB/sec    1.00   1439.8±6.01µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.1±0.62µs    19.7 MB/sec    1.00    149.9±0.48µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.1±0.02ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.6±0.45ms     2.8 MB/sec    1.01     14.8±0.40ms     2.8 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.9±0.13ms     5.6 MB/sec    1.00      2.8±0.11ms     5.9 MB/sec
formatter/numpy/globals.py                 1.01   332.9±19.15µs     8.9 MB/sec    1.00   328.7±12.68µs     9.0 MB/sec
formatter/pydantic/types.py                1.00      6.2±0.23ms     4.1 MB/sec    1.01      6.2±0.17ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     20.9±0.68ms  1996.4 KB/sec    1.00     20.8±0.49ms  2000.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.12ms     3.1 MB/sec    1.00      5.4±0.16ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   640.5±17.60µs     4.6 MB/sec    1.02   651.1±17.25µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.4±0.25ms     2.7 MB/sec    1.01      9.4±0.27ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.21ms     3.7 MB/sec    1.02     11.2±0.21ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.15ms     7.2 MB/sec    1.00      2.3±0.06ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   269.1±10.22µs    11.0 MB/sec    1.01    270.6±8.72µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.11ms     5.3 MB/sec    1.03      4.9±0.12ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-19 22:12_

---

_Closed by @charliermarsh on 2023-07-19 22:12_

---

_Label `documentation` added by @charliermarsh on 2023-07-19 22:12_

---

_Branch deleted on 2023-07-20 11:10_

---
