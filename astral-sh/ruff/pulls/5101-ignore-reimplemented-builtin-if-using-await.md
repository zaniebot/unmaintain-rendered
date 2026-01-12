```yaml
number: 5101
title: "Ignore `reimplemented-builtin` if using `await`"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: sim110-ignore-await
created_at: 2023-06-14T21:03:52Z
updated_at: 2023-07-10T09:55:26Z
url: https://github.com/astral-sh/ruff/pull/5101
synced_at: 2026-01-12T03:36:54Z
```

# Ignore `reimplemented-builtin` if using `await`

---

_Pull request opened by @tjkuson on 2023-06-14 21:03_

## Summary

Checks if `checker` is in an `async` context. If yes, return early.

Fixes #5098.

## Test Plan

`cargo test`

---

_Marked ready for review by @tjkuson on 2023-06-14 21:05_

---

_@charliermarsh reviewed on 2023-06-14 21:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/reimplemented_builtin.rs`:236 on 2023-06-14 21:10_

Could we instead check if the statement body contains an `await`? We have a `contains_await` utility in `rules/pyupgrade/rules/unpacked_list_comprehension.rs` that we could move to `helpers.rs`.

E.g., sometimes, it's okay to run this in an async function:

```py
async def f():
    # OK
    for x in iterable:
        if check(x):
            return True
    return False
```

---

_@tjkuson reviewed on 2023-06-14 21:12_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_simplify/rules/reimplemented_builtin.rs`:236 on 2023-06-14 21:12_

Yeah, that makes sense. The PR is a bit nuclear.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/reimplemented_builtin.rs`:236 on 2023-06-14 21:13_

Oh wait, we also have `is_async_generator` which is significantly simpler in `rules/flake8_comprehensions/rules/unnecessary_comprehension_any_all.rs`. We should be able to reuse that snippet (but we can just redefine it here).

---

_@charliermarsh reviewed on 2023-06-14 21:13_

---

_Comment by @github-actions[bot] on 2023-06-14 21:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.05ms     6.5 MB/sec    1.01      6.3±0.03ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1323.0±5.58µs    12.6 MB/sec    1.00   1321.7±2.54µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    126.8±0.40µs    23.3 MB/sec    1.00    126.8±0.21µs    23.3 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.9 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.15ms     3.0 MB/sec    1.02     14.0±0.21ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.5±2.44µs     7.0 MB/sec    1.01    424.9±0.74µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.06ms     4.4 MB/sec    1.01      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.1 MB/sec    1.01      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.1±2.40µs    11.4 MB/sec    1.00   1466.5±3.29µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.6±0.72µs    17.8 MB/sec    1.00    166.0±0.55µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.0±0.05ms     5.1 MB/sec    1.00      7.9±0.07ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1612.5±11.93µs    10.3 MB/sec    1.00  1611.2±19.59µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    156.8±2.62µs    18.8 MB/sec    1.01    157.6±2.71µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.04ms     7.8 MB/sec    1.00      3.3±0.10ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.33ms     2.5 MB/sec    1.05     17.2±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.05      4.4±0.03ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.6±5.45µs     6.8 MB/sec    1.04    451.2±8.27µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.19ms     3.5 MB/sec    1.02      7.4±0.12ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.04ms     4.8 MB/sec    1.02      8.6±0.10ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1769.3±12.38µs     9.4 MB/sec    1.01  1778.5±14.06µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.6±1.79µs    15.6 MB/sec    1.02    191.6±2.45µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.6 MB/sec    1.01      3.9±0.02ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-14 22:00_

---

_Closed by @charliermarsh on 2023-06-14 22:00_

---

_Comment by @charliermarsh on 2023-06-14 22:00_

Thanks!

---

_Renamed from "Ignore `reimplemented-builtin` if in `async` context" to "Ignore `reimplemented-builtin` if using `await`" by @tjkuson on 2023-06-14 22:01_

---

_Branch deleted on 2023-07-10 09:55_

---
