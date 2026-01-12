```yaml
number: 3718
title: "Allow aliased `logging` module as a logger candidate"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/detect-logging-alias
created_at: 2023-03-24T18:46:52Z
updated_at: 2023-04-04T16:22:22Z
url: https://github.com/astral-sh/ruff/pull/3718
synced_at: 2026-01-12T15:55:13Z
```

# Allow aliased `logging` module as a logger candidate

---

_@dhruvmanila_

Currently, `is_logger_candidate` will only detect if the last part of the call path but before the level function is `log`, `logger` or `logging`. This expands so that the direct calls from the `logging` module are considered even if they're aliased to names other than the ones listed above.

This affects the following rules:
- `BLE001`
- All of `flake8-logging-format` rules as there's a single entry point for every rule
- `PLE1205`
- `PLE1206`

## Test plan

A test case has been added for `G001` where the `logging` module is aliased.

---

_Comment by @github-actions[bot] on 2023-03-24 18:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.09ms     2.8 MB/sec    1.00     14.5±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.02ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.9±1.34µs     6.8 MB/sec    1.01    438.4±1.98µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.04ms     4.0 MB/sec    1.00      6.4±0.05ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.03ms     5.2 MB/sec    1.01      7.9±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1713.0±5.98µs     9.7 MB/sec    1.00   1705.9±2.64µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.4±1.05µs    16.7 MB/sec    1.00    174.4±1.72µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     7.0 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.05ms     2.7 MB/sec    1.03     15.4±0.06ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.01      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.5±7.64µs     6.5 MB/sec    1.00    456.6±5.25µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.02ms     3.8 MB/sec    1.03      6.8±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.03      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1745.0±10.71µs     9.5 MB/sec    1.02  1785.2±46.78µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.9±0.89µs    16.3 MB/sec    1.01    182.6±4.59µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.8 MB/sec    1.03      3.9±0.03ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-03-24 21:19_

---

_Merged by @charliermarsh on 2023-03-24 21:19_

---

_Closed by @charliermarsh on 2023-03-24 21:19_

---

_Comment by @charliermarsh on 2023-03-24 21:19_

👍 Makes sense to me!

---

_Branch deleted on 2023-03-25 01:48_

---

_Comment by @henryiii on 2023-04-04 14:47_

I wonder if this is why Ruff is now incorrectly changing (via `G010`) this:

```python
from distutils import log as distutils_log
distutils_log.warn(msg, *args)  # pylint: disable=deprecated-method
```

into `.warning`, which is not defined on `distutils.log` (it's not a logger from logging).

This was in scikit-build, so surprised it wasn't shown in the ecosystem check. https://github.com/scikit-build/scikit-build/pull/909

---

_Comment by @charliermarsh on 2023-04-04 14:49_

We should omit `distuils.log.warn`.

---

_Comment by @charliermarsh on 2023-04-04 14:55_

(Also unsure why it didn't appear in the ecosystem check.)

---

_Comment by @henryiii on 2023-04-04 15:15_

Not exactly critical (people shouldn't be using `distutils.log` except for very special cases, don't mind working around it), but not showing up in the ecosystem check is a bit more worrisome. 

---

_Comment by @dhruvmanila on 2023-04-04 16:22_

Oh interesting! Yes, we should check if the resolved path is from the `logging` module only.

---
