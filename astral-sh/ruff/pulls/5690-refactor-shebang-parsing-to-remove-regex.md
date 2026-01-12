```yaml
number: 5690
title: Refactor shebang parsing to remove regex dependency
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/shebang
created_at: 2023-07-11T16:16:31Z
updated_at: 2023-07-11T20:30:39Z
url: https://github.com/astral-sh/ruff/pull/5690
synced_at: 2026-01-12T03:36:55Z
```

# Refactor shebang parsing to remove regex dependency

---

_Pull request opened by @charliermarsh on 2023-07-11 16:16_

## Summary

Similar to #5567, we can remove the use of regex, plus simplify the representation (use `Option`), add snapshot tests, etc.

This is about 100x faster than using a regex for cases that match (2.5ns vs. 250ns). It's obviously not a hot path, but I prefer the consistency with other similar comment-parsing. I may DRY these up into some common functionality later on.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-11 16:16_

---

_Comment by @github-actions[bot] on 2023-07-11 16:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.01ms     4.9 MB/sec    1.01      8.5±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1894.5±2.90µs     8.8 MB/sec    1.01   1908.7±2.98µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    204.1±0.65µs    14.5 MB/sec    1.01    205.8±0.57µs    14.3 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.00ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.00     14.0±0.21ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    365.3±1.15µs     8.1 MB/sec    1.00    361.9±0.96µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1490.8±1.82µs    11.2 MB/sec    1.00   1469.6±3.58µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.03    160.7±1.36µs    18.4 MB/sec    1.00    156.2±0.25µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.7±0.32ms     3.5 MB/sec    1.00     11.6±0.37ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.18ms     6.2 MB/sec    1.00      2.7±0.12ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   300.0±22.02µs     9.8 MB/sec    1.02   305.0±25.74µs     9.7 MB/sec
formatter/pydantic/types.py                1.01      5.9±0.27ms     4.3 MB/sec    1.00      5.8±0.19ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     20.3±0.57ms     2.0 MB/sec    1.02     20.7±0.63ms  2016.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.19ms     3.2 MB/sec    1.02      5.4±0.29ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   629.2±22.60µs     4.7 MB/sec    1.01   633.4±27.78µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.36ms     2.8 MB/sec    1.00      9.2±0.30ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.36ms     4.0 MB/sec    1.01     10.4±0.29ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.6 MB/sec    1.00      2.2±0.08ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   254.8±16.58µs    11.6 MB/sec    1.00   255.9±10.53µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.16ms     5.6 MB/sec    1.02      4.6±0.15ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_executable/helpers.rs`:47 on 2023-07-11 20:29_

The Cursor from the formatters SimpleTokenizer could be useful here because it provides these helpers already (with implementations that lower well to llvm bytecode)

---

_@MichaReiser approved on 2023-07-11 20:29_

---

_@charliermarsh reviewed on 2023-07-11 20:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_executable/helpers.rs`:47 on 2023-07-11 20:30_

I'll probably replace this and the noqa parser with that.

---

_Merged by @charliermarsh on 2023-07-11 20:30_

---

_Closed by @charliermarsh on 2023-07-11 20:30_

---

_Branch deleted on 2023-07-11 20:30_

---
