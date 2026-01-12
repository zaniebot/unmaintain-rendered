```yaml
number: 5060
title: "Don't treat annotations as resolved in forward references"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/forward
created_at: 2023-06-13T18:18:39Z
updated_at: 2023-06-13T18:52:03Z
url: https://github.com/astral-sh/ruff/pull/5060
synced_at: 2026-01-12T03:43:30Z
```

# Don't treat annotations as resolved in forward references

---

_Pull request opened by @charliermarsh on 2023-06-13 18:18_

## Summary

This behavior dates back to a Pyflakes commit (5fc37cbd), which was used to allow this test to pass:

```py
from __future__ import annotations
T: object
def f(t: T): pass
def g(t: 'T'): pass
```

But, I think this is an error. Mypy and Pyright don't accept it -- you can only use variables as type annotations if they're type aliases (i.e., annotated with `TypeAlias`), in which case, there has to be an assignment on the right-hand side (see: [PEP 613](https://peps.python.org/pep-0613/)).


---

_Comment by @github-actions[bot] on 2023-06-13 18:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.01ms     6.4 MB/sec    1.02      6.5±0.06ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1322.2±6.91µs    12.6 MB/sec    1.01   1335.2±2.06µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    125.9±0.25µs    23.4 MB/sec    1.01    127.6±1.01µs    23.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.8 MB/sec    1.01      2.6±0.04ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.8±0.06ms     3.0 MB/sec    1.00     13.7±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.4±0.71µs     7.0 MB/sec    1.00    420.2±1.22µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1477.0±2.34µs    11.3 MB/sec    1.00   1471.4±2.70µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.7±0.46µs    17.8 MB/sec    1.00    165.5±0.57µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.05ms     5.1 MB/sec    1.00      7.9±0.11ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1611.3±14.80µs    10.3 MB/sec    1.00  1615.2±13.43µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    156.2±1.91µs    18.9 MB/sec    1.01    157.3±2.52µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.05ms     7.9 MB/sec    1.01      3.3±0.06ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.02     17.4±0.32ms     2.3 MB/sec    1.00     17.1±0.29ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.11ms     3.7 MB/sec    1.00      4.4±0.08ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   456.4±16.81µs     6.5 MB/sec    1.00    448.0±6.06µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.7±0.17ms     3.3 MB/sec    1.00      7.4±0.07ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.06ms     4.7 MB/sec    1.00      8.6±0.09ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1790.4±22.05µs     9.3 MB/sec    1.00  1794.9±25.10µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    190.5±1.88µs    15.5 MB/sec    1.00    189.1±2.38µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.06ms     6.6 MB/sec    1.00      3.8±0.02ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-13 18:47_

I guess I could go either way on this one... In the above example, `T` _is_ resolved to the `T` in `T: object`, so it's not _undefined_ -- but it's not bound to a "useable" (?) value. And the current behavior arguably obscures a real error, since the above will pass silently for invalid type annotations.

The comparison would be:

```py
def f():
    x: int
    print(x)
```

In this case, Python throws an `UnboundLocalError` (instead of a `NameError`, which is what you get if you omit `x: int`). In both cases, Pyflakes and Ruff output `undefined-name`. So Pyflakes omits the same error despite the fact that the semantics are a bit different when including or excluding `x: int`... Which is effectively what's being proposed here: use the same error (`undefined-name`) regardless of whether the variable is resolved to a useable value.

Anyway, I'm hoping that this is an edge case, and like I said, I can't think of _valid_ code that would trigger the new `undefined-name` errors that this will raise.


---

_Merged by @charliermarsh on 2023-06-13 18:47_

---

_Closed by @charliermarsh on 2023-06-13 18:47_

---

_Branch deleted on 2023-06-13 18:47_

---
