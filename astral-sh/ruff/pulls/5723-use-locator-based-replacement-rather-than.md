```yaml
number: 5723
title: Use Locator-based replacement rather than Generator for UP007
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/up-locator
created_at: 2023-07-13T03:44:17Z
updated_at: 2023-07-13T04:10:20Z
url: https://github.com/astral-sh/ruff/pull/5723
synced_at: 2026-01-12T15:55:19Z
```

# Use Locator-based replacement rather than Generator for UP007

---

_@charliermarsh_

## Summary

Locator-based replacement is generally preferable as we get verbatim fixes.

---

_Label `internal` added by @charliermarsh on 2023-07-13 03:44_

---

_Merged by @charliermarsh on 2023-07-13 03:50_

---

_Closed by @charliermarsh on 2023-07-13 03:50_

---

_Branch deleted on 2023-07-13 03:50_

---

_Comment by @github-actions[bot] on 2023-07-13 03:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13     10.3±0.15ms     3.9 MB/sec    1.00      9.1±0.20ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.3±0.08ms     7.2 MB/sec    1.00      2.2±0.02ms     7.5 MB/sec
formatter/numpy/globals.py                 1.01    249.7±7.16µs    11.8 MB/sec    1.00    246.1±4.27µs    12.0 MB/sec
formatter/pydantic/types.py                1.05      5.0±0.12ms     5.1 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.21ms     2.6 MB/sec    1.00     15.5±0.38ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.10ms     4.2 MB/sec    1.00      3.9±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.02    503.1±8.09µs     5.9 MB/sec    1.00   491.6±13.76µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.0±0.10ms     3.7 MB/sec    1.00      6.8±0.14ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      7.9±0.10ms     5.2 MB/sec    1.00      7.6±0.28ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1730.3±33.38µs     9.6 MB/sec    1.00  1684.8±43.11µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    199.2±2.31µs    14.8 MB/sec    1.00    195.5±4.61µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.07ms     7.1 MB/sec    1.00      3.6±0.07ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.09ms     4.3 MB/sec    1.00      9.5±0.08ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.01      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    240.4±4.28µs    12.3 MB/sec    1.02   245.7±14.22µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.01      4.8±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.15ms     2.5 MB/sec    1.00     16.1±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.1±7.97µs     5.9 MB/sec    1.00    500.1±9.02µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.08ms     3.6 MB/sec    1.00      7.2±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1716.7±17.47µs     9.7 MB/sec    1.01  1728.5±20.31µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.1±4.73µs    14.7 MB/sec    1.00    201.0±4.36µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
