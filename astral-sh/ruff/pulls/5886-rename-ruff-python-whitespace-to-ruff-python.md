```yaml
number: 5886
title: "Rename `ruff_python_whitespace` to `ruff_python_trivia`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/crate
created_at: 2023-07-19T15:27:57Z
updated_at: 2023-07-19T16:08:00Z
url: https://github.com/astral-sh/ruff/pull/5886
synced_at: 2026-01-12T03:30:22Z
```

# Rename `ruff_python_whitespace` to `ruff_python_trivia`

---

_Pull request opened by @charliermarsh on 2023-07-19 15:27_

## Summary

This crate now contains utilities for dealing with trivia more broadly: whitespace, newlines, "simple" trivia lexing, etc. So renaming it to reflect its increased responsibilities.

To avoid conflicts, I've also renamed `Token` and `TokenKind` to `SimpleToken` and `SimpleTokenKind`.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-19 15:27_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-07-19 15:27_

---

_@konstin approved on 2023-07-19 15:31_

Right when i was changing some of the token usage, positively surprised you didn't get any merge conflicts

---

_@dhruvmanila approved on 2023-07-19 15:35_

LGTM!

---

_Comment by @github-actions[bot] on 2023-07-19 15:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     11.2±0.04ms     3.6 MB/sec    1.00      9.8±0.04ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.13      2.1±0.01ms     7.8 MB/sec    1.00   1893.2±3.42µs     8.8 MB/sec
formatter/numpy/globals.py                 1.08    223.3±1.69µs    13.2 MB/sec    1.00    205.9±0.39µs    14.3 MB/sec
formatter/pydantic/types.py                1.13      4.7±0.01ms     5.4 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.09ms     3.0 MB/sec    1.00     13.6±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    370.2±1.00µs     8.0 MB/sec    1.00    367.7±2.27µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.01      7.1±0.04ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1427.6±5.25µs    11.7 MB/sec    1.00   1434.5±5.53µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.6±0.74µs    19.6 MB/sec    1.00    150.1±0.35µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.01      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.2±0.55ms     3.3 MB/sec    1.17     14.3±0.68ms     2.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.11ms     7.0 MB/sec    1.16      2.8±0.15ms     6.0 MB/sec
formatter/numpy/globals.py                 1.00   269.8±28.19µs    10.9 MB/sec    1.17   315.9±22.20µs     9.3 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.27ms     4.9 MB/sec    1.12      5.8±0.22ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.2±0.85ms     2.2 MB/sec    1.06     19.3±0.96ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.24ms     3.4 MB/sec    1.06      5.2±0.24ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   604.5±38.68µs     4.9 MB/sec    1.04   626.6±29.26µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.44ms     3.0 MB/sec    1.03      8.7±0.43ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.55ms     4.3 MB/sec    1.08     10.2±0.32ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1926.0±76.41µs     8.6 MB/sec    1.14      2.2±0.14ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   232.4±20.92µs    12.7 MB/sec    1.10   255.1±12.46µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.30ms     6.2 MB/sec    1.15      4.7±0.70ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-19 15:48_

---

_Closed by @charliermarsh on 2023-07-19 15:48_

---

_Branch deleted on 2023-07-19 15:48_

---
