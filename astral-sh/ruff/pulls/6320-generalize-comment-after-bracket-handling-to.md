```yaml
number: 6320
title: Generalize comment-after-bracket handling to lists, sets, etc.
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/list
created_at: 2023-08-03T22:51:49Z
updated_at: 2023-08-07T07:52:46Z
url: https://github.com/astral-sh/ruff/pull/6320
synced_at: 2026-01-12T15:55:21Z
```

# Generalize comment-after-bracket handling to lists, sets, etc.

---

_@charliermarsh_

## Summary

We already support preserving the end-of-line comment in calls and type parameters, as in:

```python
foo(  # comment
    bar,
)
```

This PR adds the same behavior for lists, sets, comprehensions, etc., such that we preserve:

```python
[  # comment
    1,
    2,
    3,
]
```

And related cases.

---

_Label `formatter` added by @charliermarsh on 2023-08-03 22:52_

---

_Comment by @github-actions[bot] on 2023-08-03 23:56_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.02ms     5.2 MB/sec    1.04      8.2±0.08ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1597.0±30.11µs    10.4 MB/sec    1.02  1633.2±15.36µs    10.2 MB/sec
formatter/numpy/globals.py                 1.01    183.4±4.98µs    16.1 MB/sec    1.00    182.5±2.77µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.09ms     7.5 MB/sec    1.02      3.5±0.04ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.7±0.02ms     3.8 MB/sec    1.00     10.7±0.04ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    380.3±2.81µs     7.8 MB/sec    1.00    379.3±1.61µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.01ms     5.2 MB/sec    1.00      4.9±0.01ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1118.3±4.65µs    14.9 MB/sec    1.00   1113.4±3.29µs    15.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    125.3±1.06µs    23.5 MB/sec    1.01    127.1±3.41µs    23.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.00ms    10.9 MB/sec    1.00      2.4±0.01ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.5±0.60ms     3.2 MB/sec    1.10     13.8±0.67ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.26ms     6.6 MB/sec    1.05      2.6±0.11ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00    268.1±9.18µs    11.0 MB/sec    1.08   288.4±25.03µs    10.2 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.30ms     4.7 MB/sec    1.06      5.8±0.32ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.04     17.9±0.80ms     2.3 MB/sec    1.00     17.2±0.51ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.7±0.23ms     3.5 MB/sec    1.00      4.5±0.23ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.05   589.7±24.73µs     5.0 MB/sec    1.00   563.6±24.60µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.11      8.8±0.66ms     2.9 MB/sec    1.00      7.9±0.25ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.05      9.2±0.48ms     4.4 MB/sec    1.00      8.7±0.25ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09  1946.6±66.24µs     8.6 MB/sec    1.00  1787.3±78.68µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.03   236.0±12.13µs    12.5 MB/sec    1.00   229.4±10.47µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.3±0.18ms     5.9 MB/sec    1.00      4.0±0.20ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-08-04 00:30_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/expression/parentheses.rs`:149 on 2023-08-04 00:30_

For example?

---

_@zanieb approved on 2023-08-04 00:30_

LGTM

---

_Merged by @charliermarsh on 2023-08-04 01:28_

---

_Closed by @charliermarsh on 2023-08-04 01:28_

---

_Branch deleted on 2023-08-04 01:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:121 on 2023-08-07 07:51_

Nit: We so far preferred to have `with_*` methods on the builders rather than having many dedicated builder methods

```rust
parenthesized("(", &expr.format(), ")").with_dangling_comments(comments)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:156 on 2023-08-07 07:52_

Nit: Do we need this `group`? I believe it always expands because `dangling_comments` renders a hard line break or empty line.

---

_@MichaReiser reviewed on 2023-08-07 07:52_

---
