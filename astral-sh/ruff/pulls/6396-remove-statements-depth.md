```yaml
number: 6396
title: "Remove `Statements#depth`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/depth
created_at: 2023-08-07T16:11:25Z
updated_at: 2023-08-14T21:01:02Z
url: https://github.com/astral-sh/ruff/pull/6396
synced_at: 2026-01-12T02:52:04Z
```

# Remove `Statements#depth`

---

_Pull request opened by @charliermarsh on 2023-08-07 16:11_

## Summary

This PR removes the depth tracking in `Statements`, in favor of just checking against the ancestors iterators directly in `common_ancestor`. Specifically, given two nodes `left` and `right`, we take the ancestors of `right`, then iterate over the ancestors of `left`, and stop as soon as we find an ancestor of `left` that's also an ancestor of `right`.

IIUC the computational complexity here is worse, since we now have a quadratic check in comparing two vectors... although we now store less data, shorter stacks (no recursion), etc., and I doubt this is a common enough operation to show up in benchmarking? This only runs when we have shadowed variables, and the shadowed variable is unused.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-07 16:11_

---

_Label `internal` added by @charliermarsh on 2023-08-07 16:11_

---

_Comment by @charliermarsh on 2023-08-07 16:11_

I don't feel strongly on this one :)

---

_Review request for @MichaReiser removed by @MichaReiser on 2023-08-07 16:15_

---

_Comment by @github-actions[bot] on 2023-08-07 16:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.6±0.40ms     3.9 MB/sec    1.00     10.4±0.33ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.06ms     8.2 MB/sec    1.00      2.0±0.08ms     8.2 MB/sec
formatter/numpy/globals.py                 1.01    235.0±8.70µs    12.6 MB/sec    1.00   232.5±10.66µs    12.7 MB/sec
formatter/pydantic/types.py                1.01      4.4±0.16ms     5.8 MB/sec    1.00      4.3±0.18ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.48ms     2.9 MB/sec    1.00     14.0±0.57ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.10ms     4.6 MB/sec    1.02      3.6±0.26ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   509.1±13.66µs     5.8 MB/sec    1.00   511.3±22.84µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.19ms     4.1 MB/sec    1.01      6.3±0.21ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.23ms     5.7 MB/sec    1.01      7.2±0.22ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1499.6±57.36µs    11.1 MB/sec    1.02  1530.0±87.48µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.03   186.1±25.71µs    15.9 MB/sec    1.00    180.5±6.36µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.19ms     8.1 MB/sec    1.00      3.2±0.10ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.25ms     4.1 MB/sec    1.00      9.9±0.14ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1935.1±31.85µs     8.6 MB/sec    1.00  1907.4±23.48µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    213.3±5.49µs    13.8 MB/sec    1.01   215.4±11.32µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.1 MB/sec    1.00      4.2±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.16ms     3.2 MB/sec    1.01     13.0±0.22ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.00      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.4±5.48µs     6.8 MB/sec    1.00   429.3±10.96µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.10ms     4.3 MB/sec    1.00      5.9±0.11ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.08ms     5.9 MB/sec    1.01      6.9±0.12ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1404.5±18.33µs    11.9 MB/sec    1.00  1409.4±18.29µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    159.6±5.47µs    18.5 MB/sec    1.00    158.7±2.81µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.04ms     8.4 MB/sec    1.00      3.0±0.04ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-08 10:41_

---

_Comment by @konstin on 2023-08-08 10:42_

it's a bit different part of the code, but i think we could drop the `Vec`s in `alternatives()` by e.g. returning an index of the branch and checking if the index is identical for left and right.

---

_Closed by @charliermarsh on 2023-08-14 21:01_

---
