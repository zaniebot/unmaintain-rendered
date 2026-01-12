```yaml
number: 6687
title: "Fix typo in `ruff_python_formatter` documentation"
type: pull_request
state: merged
author: LaBatata101
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-doc-typo
created_at: 2023-08-18T22:05:15Z
updated_at: 2023-08-25T20:45:49Z
url: https://github.com/astral-sh/ruff/pull/6687
synced_at: 2026-01-12T02:45:38Z
```

# Fix typo in `ruff_python_formatter` documentation

---

_Pull request opened by @LaBatata101 on 2023-08-18 22:05_

## Summary

In the documentation was written `Javascript` but we are working with `Python` here :)

## Test Plan

n/a


---

_Renamed from "Fix typo in documentation" to "Fix typo in `ruff_python_formatter` documentation" by @LaBatata101 on 2023-08-18 22:06_

---

_Comment by @github-actions[bot] on 2023-08-18 22:39_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.07ms    10.7 MB/sec    1.00      3.8±0.17ms    10.6 MB/sec
formatter/numpy/ctypeslib.py               1.01   803.6±27.92µs    20.7 MB/sec    1.00   795.7±23.64µs    20.9 MB/sec
formatter/numpy/globals.py                 1.00     83.1±5.35µs    35.5 MB/sec    1.03     85.6±3.19µs    34.5 MB/sec
formatter/pydantic/types.py                1.01  1568.3±49.42µs    16.3 MB/sec    1.00  1549.8±51.37µs    16.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.23ms     3.2 MB/sec    1.01     13.0±0.14ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.06ms     4.8 MB/sec    1.01      3.5±0.08ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   502.5±21.11µs     5.9 MB/sec    1.00   498.7±14.13µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.15ms     3.8 MB/sec    1.01      6.8±0.21ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.07ms     6.1 MB/sec    1.04      6.9±0.08ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1463.3±24.36µs    11.4 MB/sec    1.03  1500.3±41.60µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    192.1±5.77µs    15.4 MB/sec    1.00    186.0±4.72µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.06ms     8.5 MB/sec    1.04      3.1±0.06ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.5±0.17ms     9.0 MB/sec    1.00      4.5±0.19ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   917.4±32.04µs    18.2 MB/sec    1.00   914.4±32.88µs    18.2 MB/sec
formatter/numpy/globals.py                 1.01     90.5±4.34µs    32.6 MB/sec    1.00     89.6±2.68µs    32.9 MB/sec
formatter/pydantic/types.py                1.00  1837.2±44.80µs    13.9 MB/sec    1.00  1837.4±52.04µs    13.9 MB/sec
linter/all-rules/large/dataset.py          1.01     16.5±0.46ms     2.5 MB/sec    1.00     16.4±0.51ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.6±0.12ms     3.6 MB/sec    1.00      4.6±0.11ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   578.1±17.86µs     5.1 MB/sec    1.01   585.8±22.19µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.7±0.30ms     2.9 MB/sec    1.00      8.5±0.39ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03      9.4±0.39ms     4.3 MB/sec    1.00      9.1±0.27ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1968.0±56.69µs     8.5 MB/sec    1.00  1964.8±52.62µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.04   251.1±37.89µs    11.8 MB/sec    1.00    241.9±8.26µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.11ms     6.2 MB/sec    1.01      4.2±0.15ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-18 23:16_

---

_Closed by @charliermarsh on 2023-08-18 23:16_

---

_Comment by @charliermarsh on 2023-08-18 23:16_

Thanks :)

---

_Label `documentation` added by @charliermarsh on 2023-08-18 23:16_

---

_Branch deleted on 2023-08-25 20:45_

---
