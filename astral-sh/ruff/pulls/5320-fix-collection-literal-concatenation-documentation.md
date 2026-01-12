```yaml
number: 5320
title: "Fix `collection-literal-concatenation` documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-docs
created_at: 2023-06-22T22:23:51Z
updated_at: 2023-07-10T09:55:15Z
url: https://github.com/astral-sh/ruff/pull/5320
synced_at: 2026-01-12T03:36:54Z
```

# Fix `collection-literal-concatenation` documentation

---

_Pull request opened by @tjkuson on 2023-06-22 22:23_

## Summary

Move `collection-literal-concatenation` markdown documentation to the correct place.

Fixes error in #5262.

## Test Plan

`python scripts/check_docs_formatted.py`

---

_Comment by @github-actions[bot] on 2023-06-22 22:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      8.1±0.05ms     5.0 MB/sec    1.00      7.9±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.02   1776.5±9.51µs     9.4 MB/sec    1.00  1748.7±17.34µs     9.5 MB/sec
formatter/numpy/globals.py                 1.01    196.8±1.03µs    15.0 MB/sec    1.00    195.3±8.73µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.08ms     2.6 MB/sec    1.00     15.7±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    515.9±7.16µs     5.7 MB/sec    1.00    507.7±1.11µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.08ms     3.6 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.03ms     5.1 MB/sec    1.00      7.9±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1745.7±2.44µs     9.5 MB/sec    1.00   1745.3±2.10µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.7±1.36µs    15.0 MB/sec    1.00    196.0±0.44µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.3±0.20ms     4.4 MB/sec    1.00      9.2±0.22ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.06ms     8.2 MB/sec    1.00  1993.8±77.18µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    226.7±9.37µs    13.0 MB/sec    1.02   230.2±15.09µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.12ms     5.4 MB/sec    1.01      4.7±0.15ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.6±0.32ms     2.2 MB/sec    1.01     18.7±0.32ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.15ms     3.4 MB/sec    1.00      4.9±0.11ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   604.4±14.12µs     4.9 MB/sec    1.00   607.3±27.83µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.6±0.27ms     3.0 MB/sec    1.00      8.3±0.21ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.18ms     4.2 MB/sec    1.00      9.6±0.19ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.05ms     8.0 MB/sec    1.00      2.1±0.05ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    242.3±8.77µs    12.2 MB/sec    1.00    241.7±8.36µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.14ms     5.8 MB/sec    1.00      4.4±0.15ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-22 22:37_

Oof, did I do this accidentally? Sorry!

---

_Merged by @charliermarsh on 2023-06-22 22:37_

---

_Closed by @charliermarsh on 2023-06-22 22:37_

---

_Label `documentation` added by @charliermarsh on 2023-06-22 22:37_

---

_Comment by @tjkuson on 2023-06-22 22:55_

Nope, this was my fault - I'll be more careful next time!

---

_Branch deleted on 2023-07-10 09:55_

---
