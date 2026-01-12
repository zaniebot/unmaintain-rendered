```yaml
number: 4752
title: Ignore __setattr__ in FBT003
type: pull_request
state: merged
author: alexfikl
labels: []
assignees: []
merged: true
base: main
head: fix-fbt003-setattr
created_at: 2023-05-31T11:13:21Z
updated_at: 2023-05-31T15:35:48Z
url: https://github.com/astral-sh/ruff/pull/4752
synced_at: 2026-01-12T03:50:03Z
```

# Ignore __setattr__ in FBT003

---

_Pull request opened by @alexfikl on 2023-05-31 11:13_

## Summary

This is a fairly common pattern
```
object.__setattr__(self, "flag", True)
```
that's not covered by the current special case for `setattr`.

## Test Plan

Added a little test for this in `FBT.py`.


---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/flake8_boolean_trap/FBT.py`:72 on 2023-05-31 11:22_

Python noob here. Is this a realistic example? Why not use `setattr(object, "flag", true)` or `object.flag = True`? If not, can we add a realistic example where the use of `__setattr__` over `setattr` is recommended?

---

_@MichaReiser reviewed on 2023-05-31 11:22_

---

_@alexfikl reviewed on 2023-05-31 11:30_

---

_Review comment by @alexfikl on `crates/ruff/resources/test/fixtures/flake8_boolean_trap/FBT.py`:72 on 2023-05-31 11:30_

In that example, `object` would be the python base class for all classes, so basically
```
object.__setattr__(self, "flag", True)
```
will call the implementation of `setattr` of the `object` class, while
```
self.flag = True
setattr(self, "flag", True)
self.__setattr__(self, "flag", True)
```
all call the implementation of the `self` class (here that's `Registry`). This is sometimes used when you want to bypass any custom user implementation of `__setattr__` and just call the default from `object`.

We can add another class with an overwritten `__setattr__` to highlight the difference, but not sure it's worth it (?).

---

_@alexfikl reviewed on 2023-05-31 11:36_

---

_Review comment by @alexfikl on `crates/ruff/resources/test/fixtures/flake8_boolean_trap/FBT.py`:72 on 2023-05-31 11:36_

For my real-life example, I used this pattern to memoize some properties on a frozen [dataclass](https://docs.python.org/3/library/dataclasses.html) and called `object.__setattr__(self, "cache_dict", {})`, which would be impossible with `setattr`. (maybe debatable how hacky that is?)

---

_Comment by @github-actions[bot] on 2023-05-31 11:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.06ms     2.7 MB/sec    1.01     15.3±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.02ms     4.5 MB/sec    1.00      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.3±1.46µs     7.9 MB/sec    1.00    374.9±2.47µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.5 MB/sec    1.01      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1591.0±3.99µs    10.5 MB/sec    1.01   1600.6±4.78µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.1±0.28µs    17.1 MB/sec    1.01    174.4±0.20µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.7±0.00ms     7.1 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.02   1139.0±1.11µs    14.6 MB/sec    1.00   1121.6±1.63µs    14.8 MB/sec
parser/numpy/globals.py                    1.01    115.8±0.38µs    25.5 MB/sec    1.00    114.6±0.37µs    25.7 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.3 MB/sec    1.00      2.4±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.0±0.20ms     2.4 MB/sec    1.00     16.7±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.7±7.11µs     5.9 MB/sec    1.01    505.2±6.74µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     4.9 MB/sec    1.01      8.3±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1762.8±16.92µs     9.4 MB/sec    1.02  1789.5±21.65µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.8±4.60µs    14.5 MB/sec    1.01    205.2±6.04µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.3 MB/sec    1.01      6.5±0.08ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1215.2±18.33µs    13.7 MB/sec    1.01  1229.0±13.60µs    13.5 MB/sec
parser/numpy/globals.py                    1.01    126.4±2.75µs    23.3 MB/sec    1.00    125.5±2.40µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.3 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-05-31 14:36_

---

_Merged by @charliermarsh on 2023-05-31 14:36_

---

_Closed by @charliermarsh on 2023-05-31 14:36_

---

_Branch deleted on 2023-05-31 14:40_

---

_@MichaReiser reviewed on 2023-05-31 15:35_

---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/flake8_boolean_trap/FBT.py`:72 on 2023-05-31 15:35_

Thx, that helped 

---
