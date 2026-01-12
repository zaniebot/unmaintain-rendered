```yaml
number: 6712
title: Document IO Error
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-io-error
created_at: 2023-08-21T08:30:58Z
updated_at: 2023-08-22T09:46:19Z
url: https://github.com/astral-sh/ruff/pull/6712
synced_at: 2026-01-12T15:55:22Z
```

# Document IO Error

---

_@konstin_

`IOError` is special, it is not actually a lint but an error before linting. I'm not entirely sure how to document it since it does not match the general lint rule pattern (`Checks that the file can be read in its entirety.` is imho worse).

I added the in my experience two most common reasons for io errors on unix systems and linked two tutorials on how to fix them.

See https://github.com/astral-sh/ruff/issues/2646

---

_Label `documentation` added by @konstin on 2023-08-21 08:30_

---

_@konstin reviewed on 2023-08-21 08:31_

---

_Review comment by @konstin on `crates/ruff/src/rules/pycodestyle/rules/errors.rs`:19 on 2023-08-21 08:31_

Should this be `unix` or `linux and Mac OS` instead?

---

_Comment by @github-actions[bot] on 2023-08-21 08:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.0±0.04ms    10.3 MB/sec    1.00      3.9±0.03ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.01    840.2±7.48µs    19.8 MB/sec    1.00   830.2±11.61µs    20.1 MB/sec
formatter/numpy/globals.py                 1.05     90.7±1.27µs    32.5 MB/sec    1.00     86.1±1.21µs    34.3 MB/sec
formatter/pydantic/types.py                1.02  1647.2±32.30µs    15.5 MB/sec    1.00  1615.4±16.68µs    15.8 MB/sec
linter/all-rules/large/dataset.py          1.01     12.1±0.12ms     3.3 MB/sec    1.00     12.1±0.11ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.02ms     5.1 MB/sec    1.00      3.2±0.02ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    463.0±3.29µs     6.4 MB/sec    1.00    464.5±2.53µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.04ms     4.0 MB/sec    1.00      6.3±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1435.1±8.43µs    11.6 MB/sec    1.00   1420.3±8.36µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.1±1.18µs    17.8 MB/sec    1.02    169.6±2.35µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.7 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02      4.9±0.35ms     8.2 MB/sec     1.00      4.8±0.34ms     8.4 MB/sec
formatter/numpy/ctypeslib.py               1.04  1016.3±80.93µs    16.4 MB/sec     1.00   980.8±51.62µs    17.0 MB/sec
formatter/numpy/globals.py                 1.00    101.1±7.66µs    29.2 MB/sec     1.01    102.1±9.57µs    28.9 MB/sec
formatter/pydantic/types.py                1.01  1986.9±127.93µs    12.8 MB/sec    1.00  1962.7±132.26µs    13.0 MB/sec
linter/all-rules/large/dataset.py          1.04     18.3±0.78ms     2.2 MB/sec     1.00     17.6±0.62ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.8±0.25ms     3.5 MB/sec     1.00      4.8±0.26ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   589.6±46.86µs     5.0 MB/sec     1.02   602.2±27.07µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.5±0.67ms     2.7 MB/sec     1.00      9.3±0.47ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02      9.9±0.66ms     4.1 MB/sec     1.00      9.8±0.51ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.9 MB/sec     1.01      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.07   270.7±26.21µs    10.9 MB/sec     1.00   254.2±19.15µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.7±0.28ms     5.5 MB/sec     1.00      4.4±0.22ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-21 13:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/errors.rs`:19 on 2023-08-21 13:17_

Yeah maybe like `Linux or macOS`?

---

_@konstin reviewed on 2023-08-21 13:52_

---

_Review comment by @konstin on `crates/ruff/src/rules/pycodestyle/rules/errors.rs`:19 on 2023-08-21 13:52_

TIL they switched from Mac OS X to macOS in 10.12 

---

_@charliermarsh approved on 2023-08-22 01:23_

---

_Merged by @konstin on 2023-08-22 09:46_

---

_Closed by @konstin on 2023-08-22 09:46_

---

_Branch deleted on 2023-08-22 09:46_

---
