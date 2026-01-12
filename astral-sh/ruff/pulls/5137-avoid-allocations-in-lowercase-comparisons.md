```yaml
number: 5137
title: Avoid allocations in lowercase comparisons
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lower
created_at: 2023-06-16T03:23:42Z
updated_at: 2023-06-16T12:57:44Z
url: https://github.com/astral-sh/ruff/pull/5137
synced_at: 2026-01-12T15:55:17Z
```

# Avoid allocations in lowercase comparisons

---

_@charliermarsh_

## Summary

I noticed that we have a few hot comparisons that involve called `s.to_lowercase()`. We can avoid an allocation by comparing characters directly.


---

_@charliermarsh reviewed on 2023-06-16 03:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/str.rs`:64 on 2023-06-16 03:24_

These `is_cased_*` variants were the already-existing functions. I tried to differentiate them clearly from the `is_lowercase` and `is_uppercase` functions above... It's very nuanced, but we have good test coverage in the test suite and the differences matter.

---

_Review requested from @konstin by @charliermarsh on 2023-06-16 03:24_

---

_Comment by @github-actions[bot] on 2023-06-16 03:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.4 MB/sec    1.01      6.4±0.02ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1333.0±4.37µs    12.5 MB/sec    1.00   1332.9±2.65µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    129.2±0.20µs    22.8 MB/sec    1.01    130.3±0.10µs    22.6 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.8 MB/sec    1.01      2.6±0.02ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.04ms     3.0 MB/sec    1.00     13.7±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.0±0.70µs     6.9 MB/sec    1.00    426.4±0.47µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.5±3.66µs    11.3 MB/sec    1.00   1470.2±2.15µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.4±0.52µs    17.5 MB/sec    1.00    166.5±0.20µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.33ms     4.3 MB/sec    1.02      9.8±0.43ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.11ms     8.2 MB/sec    1.00      2.0±0.13ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   190.9±15.73µs    15.5 MB/sec    1.03   197.4±17.57µs    15.0 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.25ms     6.4 MB/sec    1.00      3.9±0.18ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.04     21.5±2.05ms  1934.3 KB/sec    1.00     20.7±0.72ms  2015.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.22ms     3.2 MB/sec    1.01      5.3±0.25ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   621.6±26.12µs     4.7 MB/sec    1.02   631.2±33.41µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.1±0.40ms     2.8 MB/sec    1.00      9.0±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.6±0.39ms     3.8 MB/sec    1.00     10.5±0.35ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.5 MB/sec    1.00      2.2±0.10ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.03   257.4±18.02µs    11.5 MB/sec    1.00   251.0±12.02µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.19ms     5.4 MB/sec    1.00      4.7±0.21ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Boshen on 2023-06-16 05:16_

> When a thing is "zero-allocation"? I love it. I go crazy for it. I can't get enough of it.

Got stuck in my brain.

---

_@konstin approved on 2023-06-16 07:50_

---

_Comment by @charliermarsh on 2023-06-16 12:57_

YES @Boshen! YES!!!

---

_Merged by @charliermarsh on 2023-06-16 12:57_

---

_Closed by @charliermarsh on 2023-06-16 12:57_

---

_Branch deleted on 2023-06-16 12:57_

---
