```yaml
number: 5126
title: "UP035: Deprecate `typing.Union` and `typing.NoReturn`"
type: pull_request
state: closed
author: sollyvarcoe
labels: []
assignees: []
base: main
head: deprecate-typing-noreturn-union
created_at: 2023-06-15T19:04:47Z
updated_at: 2023-07-20T22:15:57Z
url: https://github.com/astral-sh/ruff/pull/5126
synced_at: 2026-01-12T03:30:21Z
```

# UP035: Deprecate `typing.Union` and `typing.NoReturn`

---

_Pull request opened by @sollyvarcoe on 2023-06-15 19:04_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update UP035 to catch the following:

Python > 3.11
```
from typing import NoReturn
>> `typing.NoReturn` is deprecated, use `typing.Never` instead
```

Python > 3.10
```
from typing import Union
>> `typing.Union` is deprecated, use `|` instead
```

## Test Plan

Added additional fixture for 3.11 and amended existing file with updated examples

## Issue Link

Fixes: #5111 

---

_Renamed from "Deprecate `typing.Union` for py310 and `typing.NoReturn`" to "Deprecate `typing.Union` and `typing.NoReturn`" by @sollyvarcoe on 2023-06-15 19:04_

---

_Renamed from "Deprecate `typing.Union` and `typing.NoReturn`" to "UP035: Deprecate `typing.Union` and `typing.NoReturn`" by @sollyvarcoe on 2023-06-15 19:09_

---

_Comment by @github-actions[bot] on 2023-06-15 19:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1395.7±1.77µs    11.9 MB/sec    1.01   1404.0±4.11µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    138.4±0.85µs    21.3 MB/sec    1.01    139.6±0.66µs    21.1 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.02ms     9.2 MB/sec    1.01      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.09ms     2.8 MB/sec    1.01     14.5±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.01      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.8±2.13µs     8.0 MB/sec    1.01    374.0±0.91µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.05ms     4.1 MB/sec    1.00      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1512.1±4.82µs    11.0 MB/sec    1.00   1513.7±4.30µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.8±0.19µs    17.9 MB/sec    1.00    164.0±3.84µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.02ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      8.1±0.06ms     5.0 MB/sec    1.00      7.7±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.04  1631.9±16.39µs    10.2 MB/sec    1.00  1573.7±16.77µs    10.6 MB/sec
formatter/numpy/globals.py                 1.01    155.7±2.22µs    19.0 MB/sec    1.00    153.5±6.30µs    19.2 MB/sec
formatter/pydantic/types.py                1.05      3.3±0.06ms     7.7 MB/sec    1.00      3.1±0.04ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.13ms     2.5 MB/sec    1.00     16.1±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.05ms     4.0 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.5±5.17µs     5.9 MB/sec    1.01   499.5±12.03µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     4.9 MB/sec    1.00      8.2±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1754.9±30.65µs     9.5 MB/sec    1.00  1743.1±20.32µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    203.7±6.25µs    14.5 MB/sec    1.00    202.1±4.37µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @sollyvarcoe on 2023-06-15 19:47_

---

_Comment by @charliermarsh on 2023-06-15 20:52_

Ugh, my bad -- this is a nice PR, though on reconsideration, I'm leaning towards _not_ marking these as deprecated based on the comments in https://github.com/astral-sh/ruff/issues/5111#issuecomment-1593706211 and some additional research. What do you think?

---

_Comment by @charliermarsh on 2023-07-20 22:15_

Closing for now -- needs a decision as to how we handle this (e.g., as a separate rule).

---

_Closed by @charliermarsh on 2023-07-20 22:15_

---
