```yaml
number: 6401
title: "Avoid attempting to fix `.format(...)` calls with too-few-arguments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/format-literals
created_at: 2023-08-07T18:53:04Z
updated_at: 2023-08-07T19:39:38Z
url: https://github.com/astral-sh/ruff/pull/6401
synced_at: 2026-01-12T15:55:21Z
```

# Avoid attempting to fix `.format(...)` calls with too-few-arguments

---

_@charliermarsh_

## Summary

We can anticipate earlier that this will error, so we should avoid flagging the error at all. Specifically, we're talking about cases like `"{1} {0}".format(*args)"`, in which we'd need to reorder the arguments in order to remove the `1` and `0`, but we _can't_ reorder the arguments since they're not statically analyzable.

Closes https://github.com/astral-sh/ruff/issues/6388.


---

_Label `bug` added by @charliermarsh on 2023-08-07 18:53_

---

_Merged by @charliermarsh on 2023-08-07 19:13_

---

_Closed by @charliermarsh on 2023-08-07 19:13_

---

_Branch deleted on 2023-08-07 19:13_

---

_Comment by @github-actions[bot] on 2023-08-07 19:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.03ms     5.0 MB/sec    1.00      8.2±0.09ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1620.8±21.56µs    10.3 MB/sec    1.02  1646.4±49.22µs    10.1 MB/sec
formatter/numpy/globals.py                 1.01    182.8±5.35µs    16.1 MB/sec    1.00    181.8±4.00µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.05ms     7.4 MB/sec    1.01      3.5±0.07ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.07ms     4.0 MB/sec    1.01     10.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.00ms     6.2 MB/sec    1.01      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    379.8±2.72µs     7.8 MB/sec    1.01    383.2±1.28µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.02ms     5.5 MB/sec    1.01      4.7±0.02ms     5.4 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.8 MB/sec    1.01      5.3±0.01ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1129.1±1.73µs    14.7 MB/sec    1.01  1136.1±16.74µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    127.4±0.19µs    23.2 MB/sec    1.01    129.2±1.09µs    22.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.02ms    10.9 MB/sec    1.00      2.4±0.01ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.08ms     4.2 MB/sec    1.01      9.8±0.06ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1861.1±20.65µs     8.9 MB/sec    1.00  1857.5±11.08µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    191.2±2.62µs    15.4 MB/sec    1.01    193.4±7.98µs    15.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.1 MB/sec    1.00      4.2±0.03ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.06ms     3.2 MB/sec    1.01     12.7±0.06ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    366.5±6.99µs     8.1 MB/sec    1.00    359.1±6.47µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.04ms     6.1 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1385.0±10.78µs    12.0 MB/sec    1.00   1379.7±9.60µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    140.3±1.80µs    21.0 MB/sec    1.00    139.6±1.51µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.5 MB/sec    1.01      3.0±0.02ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
