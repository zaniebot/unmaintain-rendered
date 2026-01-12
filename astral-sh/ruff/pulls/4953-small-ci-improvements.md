```yaml
number: 4953
title: Small CI improvements
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: small_ci_improvement
created_at: 2023-06-08T10:19:24Z
updated_at: 2023-06-09T07:03:36Z
url: https://github.com/astral-sh/ruff/pull/4953
synced_at: 2026-01-12T15:55:17Z
```

# Small CI improvements

---

_@konstin_

* Replace the unmaintained actions-rs/cargo that github actions complains about using an old node version with plain cargo (this was the original motivation for this PR)
* Use taiki-e/install-action to install critcmp directly
* Use a rust 1.70 nightly toolchain for udeps
* Cache python package build (this should cut a good chunk of ci time)
* yaml formatting courtesy of pycharm

Test Plan: CI itself


---

_Comment by @github-actions[bot] on 2023-06-08 10:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.21ms     5.9 MB/sec    1.08      7.5±0.30ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1439.5±71.48µs    11.6 MB/sec    1.04  1501.3±49.35µs    11.1 MB/sec
formatter/numpy/globals.py                 1.00    163.9±7.89µs    18.0 MB/sec    1.01    165.7±8.60µs    17.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.13ms     8.0 MB/sec    1.00      3.2±0.17ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.04     18.6±1.22ms     2.2 MB/sec    1.00     17.9±0.48ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.20ms     3.9 MB/sec    1.00      4.2±0.17ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.0±21.10µs     5.6 MB/sec    1.03   545.5±29.57µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.30ms     3.4 MB/sec    1.06      7.9±0.36ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.33ms     4.7 MB/sec    1.04      9.0±0.29ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1834.7±72.56µs     9.1 MB/sec    1.04  1903.7±60.18µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   220.7±16.36µs    13.4 MB/sec    1.03   226.7±12.11µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.12ms     6.4 MB/sec    1.04      4.1±0.19ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.1±0.28ms     5.0 MB/sec    1.00      8.0±0.30ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1556.8±65.16µs    10.7 MB/sec    1.00  1547.0±87.77µs    10.8 MB/sec
formatter/numpy/globals.py                 1.04    179.8±8.80µs    16.4 MB/sec    1.00   172.9±12.86µs    17.1 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.15ms     7.2 MB/sec    1.01      3.6±0.15ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     20.3±0.58ms     2.0 MB/sec    1.00     20.2±0.61ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.1±0.19ms     3.3 MB/sec    1.00      5.0±0.19ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   582.0±24.52µs     5.1 MB/sec    1.02   593.0±29.94µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.32ms     3.0 MB/sec    1.02      8.5±0.34ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02     10.1±0.33ms     4.0 MB/sec    1.00     10.0±0.40ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.14ms     7.8 MB/sec    1.00      2.1±0.09ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.3±13.69µs    12.3 MB/sec    1.00   239.2±13.84µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.22ms     5.7 MB/sec    1.00      4.5±0.22ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-06-08 10:46_

---

_Review requested from @charliermarsh by @konstin on 2023-06-08 11:11_

---

_@charliermarsh approved on 2023-06-08 15:42_

---

_Merged by @konstin on 2023-06-09 07:03_

---

_Closed by @konstin on 2023-06-09 07:03_

---

_Branch deleted on 2023-06-09 07:03_

---
