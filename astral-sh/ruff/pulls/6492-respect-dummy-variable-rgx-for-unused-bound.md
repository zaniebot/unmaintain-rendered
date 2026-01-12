```yaml
number: 6492
title: Respect dummy-variable-rgx for unused bound exceptions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unused-exception
created_at: 2023-08-11T03:52:50Z
updated_at: 2023-08-11T04:20:14Z
url: https://github.com/astral-sh/ruff/pull/6492
synced_at: 2026-01-12T02:52:04Z
```

# Respect dummy-variable-rgx for unused bound exceptions

---

_Pull request opened by @charliermarsh on 2023-08-11 03:52_

## Summary

This PR respects our unused variable regex when flagging bound exceptions, so that you no longer get a violation for, e.g.:

```python
def f():
    try:
        pass
    except Exception as _:
        pass
```

This is an odd pattern, but I think it's surprising that the regex _isn't_ respected here.

Closes https://github.com/astral-sh/ruff/issues/6391

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-11 03:52_

---

_Marked ready for review by @charliermarsh on 2023-08-11 03:52_

---

_@charliermarsh reviewed on 2023-08-11 03:53_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__f841_dummy_variable_rgx.snap`:276 on 2023-08-11 03:53_

(This is expected -- it's a variant of the fixture where we replace the dummy variable regex with `z`.)

---

_Merged by @charliermarsh on 2023-08-11 04:02_

---

_Closed by @charliermarsh on 2023-08-11 04:02_

---

_Branch deleted on 2023-08-11 04:02_

---

_Comment by @github-actions[bot] on 2023-08-11 04:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.10ms     4.2 MB/sec    1.02     10.0±0.09ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1927.9±15.65µs     8.6 MB/sec    1.02  1956.9±10.93µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    219.0±2.97µs    13.5 MB/sec    1.01    220.4±4.75µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.03ms     6.2 MB/sec    1.01      4.2±0.04ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.11ms     3.2 MB/sec    1.00     12.5±0.17ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.04ms     5.0 MB/sec    1.00      3.3±0.03ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    471.0±3.43µs     6.3 MB/sec    1.00    472.1±2.16µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.05ms     3.9 MB/sec    1.00      6.5±0.06ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.05ms     6.2 MB/sec    1.00      6.5±0.09ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1432.8±7.47µs    11.6 MB/sec    1.01  1440.6±25.85µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.2±1.90µs    17.6 MB/sec    1.02    170.2±2.23µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.7 MB/sec    1.00      2.9±0.04ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.3±0.14ms     3.9 MB/sec    1.00     10.2±0.12ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1956.0±26.18µs     8.5 MB/sec    1.00  1938.8±28.74µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    216.3±8.57µs    13.6 MB/sec    1.01    217.9±8.31µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.07ms     5.9 MB/sec    1.01      4.4±0.11ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.15ms     3.2 MB/sec    1.01     12.7±0.19ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.8 MB/sec    1.01      3.5±0.06ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    436.8±5.51µs     6.8 MB/sec    1.00    432.3±7.81µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.6±0.14ms     3.9 MB/sec    1.00      6.5±0.10ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.14ms     5.8 MB/sec    1.01      7.1±0.12ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1460.2±17.95µs    11.4 MB/sec    1.01  1469.5±17.46µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01   172.5±11.58µs    17.1 MB/sec    1.00    170.6±3.72µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.3 MB/sec    1.01      3.1±0.04ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
