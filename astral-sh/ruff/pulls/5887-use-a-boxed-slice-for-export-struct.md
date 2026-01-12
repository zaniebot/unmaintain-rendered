```yaml
number: 5887
title: "Use a boxed slice for `Export` struct"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/boxed
created_at: 2023-07-19T15:31:29Z
updated_at: 2023-07-19T16:06:56Z
url: https://github.com/astral-sh/ruff/pull/5887
synced_at: 2026-01-12T15:55:19Z
```

# Use a boxed slice for `Export` struct

---

_@charliermarsh_

## Summary

The vector of names here is immutable -- we never push to it after initialization. Boxing reduces the size of the variant from 32 bytes to 24 bytes. (See: https://nnethercote.github.io/perf-book/type-sizes.html#boxed-slices.) It doesn't make a difference here, since it's not the largest variant, but it still seems like a prudent change (and I was considering adding another field to this variant, though I may no longer do so).


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-19 15:31_

---

_Comment by @github-actions[bot] on 2023-07-19 15:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     12.5±0.50ms     3.3 MB/sec    1.00     11.6±0.34ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.13ms     6.8 MB/sec    1.01      2.5±0.14ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   267.5±14.66µs    11.0 MB/sec    1.05   279.7±21.72µs    10.5 MB/sec
formatter/pydantic/types.py                1.01      5.2±0.24ms     4.9 MB/sec    1.00      5.2±0.22ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.0±0.94ms     2.4 MB/sec    1.09     18.5±0.64ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.17ms     3.9 MB/sec    1.12      4.8±0.52ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.02   595.7±60.10µs     5.0 MB/sec    1.00   586.5±32.27µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.50ms     3.2 MB/sec    1.05      8.3±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.33ms     4.7 MB/sec    1.19     10.2±0.42ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1815.1±78.99µs     9.2 MB/sec    1.15      2.1±0.08ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    212.2±8.91µs    13.9 MB/sec    1.11   235.6±12.89µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.16ms     6.7 MB/sec    1.16      4.5±0.17ms     5.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.06ms     3.7 MB/sec    1.17     12.8±0.05ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.13      2.4±0.02ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00    231.0±1.61µs    12.8 MB/sec    1.10    253.5±7.70µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.04ms     5.4 MB/sec    1.15      5.4±0.06ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.42ms     2.6 MB/sec    1.00     15.5±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    419.2±8.04µs     7.0 MB/sec    1.00    416.1±6.59µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.08ms     3.6 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.01      8.1±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1631.2±14.85µs    10.2 MB/sec    1.01  1640.5±10.00µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.8±1.05µs    17.2 MB/sec    1.00    171.9±1.60µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-19 15:45_

---

_Closed by @charliermarsh on 2023-07-19 15:45_

---

_Branch deleted on 2023-07-19 15:45_

---
