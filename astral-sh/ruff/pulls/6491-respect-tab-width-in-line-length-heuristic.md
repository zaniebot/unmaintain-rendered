```yaml
number: 6491
title: Respect tab width in line-length heuristic
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tabs
created_at: 2023-08-11T01:49:25Z
updated_at: 2023-08-11T02:28:26Z
url: https://github.com/astral-sh/ruff/pull/6491
synced_at: 2026-01-12T02:52:04Z
```

# Respect tab width in line-length heuristic

---

_Pull request opened by @charliermarsh on 2023-08-11 01:49_

## Summary

In https://github.com/astral-sh/ruff/pull/5811, I suggested that we add a heuristic to the overlong-lines check such that if the line had fewer bytes than the character limit, we return early -- the idea being that a single byte per character was the "worst case". I overlooked that this isn't true for tabs -- with tabs, the "worst case" scenario is that every byte is a tab, which can have a width greater than 1.

Closes https://github.com/astral-sh/ruff/issues/6425.

## Test Plan

`cargo test` with a new fixture borrowed from the issue, plus manual testing.


---

_Marked ready for review by @charliermarsh on 2023-08-11 01:49_

---

_Label `bug` added by @charliermarsh on 2023-08-11 01:49_

---

_Comment by @github-actions[bot] on 2023-08-11 02:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1631.0±10.40µs    10.2 MB/sec    1.01  1643.7±31.18µs    10.1 MB/sec
formatter/numpy/globals.py                 1.01    185.8±5.07µs    15.9 MB/sec    1.00    184.2±2.15µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.08ms     7.3 MB/sec    1.01      3.5±0.05ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.15ms     3.9 MB/sec    1.02     10.7±0.16ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.02      2.8±0.02ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    387.4±0.87µs     7.6 MB/sec    1.03    398.0±3.31µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.07ms     4.6 MB/sec    1.00      5.5±0.10ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.05ms     7.6 MB/sec    1.04      5.6±0.04ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1146.4±6.31µs    14.5 MB/sec    1.06   1215.4±9.44µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    133.2±3.37µs    22.2 MB/sec    1.07    142.7±0.89µs    20.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.02ms    10.7 MB/sec    1.03      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01     12.9±0.55ms     3.2 MB/sec     1.00     12.8±0.51ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.5±0.19ms     6.6 MB/sec     1.00      2.5±0.15ms     6.7 MB/sec
formatter/numpy/globals.py                 1.00   279.4±20.93µs    10.6 MB/sec     1.03   287.2±44.60µs    10.3 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.28ms     4.7 MB/sec     1.01      5.4±0.23ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.03     17.4±0.49ms     2.3 MB/sec     1.00     16.8±0.58ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.23ms     3.6 MB/sec     1.00      4.6±0.21ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.03   593.6±50.93µs     5.0 MB/sec     1.00   578.1±26.33µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.2±0.40ms     2.8 MB/sec     1.00      8.8±0.32ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.04      9.5±0.40ms     4.3 MB/sec     1.00      9.1±0.36ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1960.4±100.15µs     8.5 MB/sec    1.00  1919.4±67.60µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   241.2±14.20µs    12.2 MB/sec     1.01   243.4±20.64µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.41ms     6.1 MB/sec     1.00      4.1±0.30ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-11 02:28_

---

_Closed by @charliermarsh on 2023-08-11 02:28_

---

_Branch deleted on 2023-08-11 02:28_

---
