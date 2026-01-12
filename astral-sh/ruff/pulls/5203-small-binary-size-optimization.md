```yaml
number: 5203
title: Small binary size optimization
type: pull_request
state: merged
author: dosisod
labels:
  - performance
assignees: []
merged: true
base: main
head: codegen-units-flag
created_at: 2023-06-20T05:30:39Z
updated_at: 2023-06-20T08:33:53Z
url: https://github.com/astral-sh/ruff/pull/5203
synced_at: 2026-01-12T15:55:17Z
```

# Small binary size optimization

---

_@dosisod_

This is a small PR that reduces the size of stripped release binaries by about ~11%. I haven't done any perf tests (I'll let the CI system do that for me), though eliminating more code should only improve performance (one would hope).

I heard about this from optimization from the [min-sized-rust](https://github.com/johnthagen/min-sized-rust#reduce-parallel-code-generation-units-to-increase-optimization) repository.

Some stats:

| Branch            | Size           | Size (stripped) |
|-------------------|----------------|-----------------|
| `main` (4cc3cdba) | 25234904 (25M) | 15267032 (15M)  |
| This PR           | 22502240 (22M) | 13681880 (14M)  |



---

_Comment by @github-actions[bot] on 2023-06-20 05:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.10      7.5±0.01ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1382.6±8.04µs    12.0 MB/sec    1.06   1468.2±3.15µs    11.3 MB/sec
formatter/numpy/globals.py                 1.00    135.5±0.23µs    21.8 MB/sec    1.06   144.0±10.58µs    20.5 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.3 MB/sec    1.08      3.0±0.01ms     8.6 MB/sec
linter/all-rules/large/dataset.py          1.10     14.9±0.07ms     2.7 MB/sec    1.00     13.6±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.6±0.01ms     4.7 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.03    371.8±1.57µs     7.9 MB/sec    1.00    360.3±1.19µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.3±0.01ms     4.0 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.15      8.1±0.02ms     5.0 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.12   1649.7±2.94µs    10.1 MB/sec    1.00   1471.3±3.97µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.10    172.7±0.96µs    17.1 MB/sec    1.00    157.2±1.09µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.6±0.01ms     7.2 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.6±0.07ms     5.3 MB/sec    1.00      7.5±0.07ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.01  1560.9±33.72µs    10.7 MB/sec    1.00  1549.2±50.94µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    146.3±3.30µs    20.2 MB/sec    1.05    153.4±7.78µs    19.2 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.05ms     8.3 MB/sec    1.00      3.1±0.05ms     8.3 MB/sec
linter/all-rules/large/dataset.py          1.02     15.6±0.18ms     2.6 MB/sec    1.00     15.2±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.2 MB/sec    1.00      4.0±0.07ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    481.5±5.92µs     6.1 MB/sec    1.02   492.4±12.53µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.8 MB/sec    1.01      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.06ms     5.1 MB/sec    1.00      7.9±0.17ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1695.4±23.79µs     9.8 MB/sec    1.00  1692.7±19.50µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   192.5±10.22µs    15.3 MB/sec    1.02    195.6±3.57µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-06-20 06:46_

Nice!

---

_Label `performance` added by @MichaReiser on 2023-06-20 06:47_

---

_Merged by @MichaReiser on 2023-06-20 06:47_

---

_Closed by @MichaReiser on 2023-06-20 06:47_

---

_Branch deleted on 2023-06-20 08:33_

---
