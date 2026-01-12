```yaml
number: 5785
title: Remove lexer-based comment range detection
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/comments
created_at: 2023-07-15T19:30:08Z
updated_at: 2023-07-16T01:20:24Z
url: https://github.com/astral-sh/ruff/pull/5785
synced_at: 2026-01-12T03:30:21Z
```

# Remove lexer-based comment range detection

---

_Pull request opened by @charliermarsh on 2023-07-15 19:30_

## Summary

I'm doing some unrelated profiling, and I noticed that this method is actually measurable on the CPython benchmark -- it's > 1% of execution time. We don't need to lex here, we already know the ranges of all comments, so we can just do a simple binary search for overlap, which brings the method down to 0%.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-15 19:36_

---

_Comment by @github-actions[bot] on 2023-07-15 19:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.02ms     4.3 MB/sec    1.00      9.4±0.01ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1865.6±3.64µs     8.9 MB/sec    1.00   1871.9±3.38µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.2±2.25µs    14.0 MB/sec    1.00    210.2±0.25µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.00ms     6.3 MB/sec    1.01      4.1±0.00ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.05ms     3.0 MB/sec    1.00     13.5±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.7±2.29µs     6.5 MB/sec    1.00    450.2±0.69µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.00      6.0±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.00      6.7±0.06ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1471.9±3.33µs    11.3 MB/sec    1.00   1461.7±1.14µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.1±1.22µs    17.7 MB/sec    1.00    166.5±0.19µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.18ms     3.7 MB/sec    1.00     11.1±0.13ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.6 MB/sec    1.00      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   246.7±11.60µs    12.0 MB/sec    1.00    247.8±7.61µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.07ms     5.3 MB/sec    1.00      4.8±0.07ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.23ms     2.5 MB/sec    1.02     16.2±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.01      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.4±6.87µs     5.9 MB/sec    1.01    508.6±8.13µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.14ms     3.6 MB/sec    1.01      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.12ms     5.0 MB/sec    1.00      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1724.0±25.23µs     9.7 MB/sec    1.01  1743.1±30.58µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.5±6.93µs    14.6 MB/sec    1.00    202.2±3.31µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:147 on 2023-07-15 20:06_

Nit: can we move this method to comment ranges. It could also be useful in other placer where we don't have an indexer (formatter)

---

_@MichaReiser approved on 2023-07-15 20:06_

Nice find 

---

_Merged by @charliermarsh on 2023-07-16 01:03_

---

_Closed by @charliermarsh on 2023-07-16 01:03_

---

_Branch deleted on 2023-07-16 01:03_

---
