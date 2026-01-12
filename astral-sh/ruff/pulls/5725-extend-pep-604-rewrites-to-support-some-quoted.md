```yaml
number: 5725
title: Extend PEP 604 rewrites to support some quoted annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/string-option
created_at: 2023-07-13T04:11:16Z
updated_at: 2023-07-13T11:34:05Z
url: https://github.com/astral-sh/ruff/pull/5725
synced_at: 2026-01-12T15:55:19Z
```

# Extend PEP 604 rewrites to support some quoted annotations

---

_@charliermarsh_

## Summary

Python doesn't allow `"Foo" | None` if the annotation will be evaluated at runtime (see the comments in the PR, or the semantic model documentation for more on what this means and when it is true), but it _does_ allow it if the annotation is typing-only.

This, for example, is invalid, as Python will evaluate `"Foo" | None` at runtime in order to
populate the function's `__annotations__`:

```python
def f(x: "Foo" | None): ...
```

This, however, is valid:

```python
def f():
    x: "Foo" | None
```

As is this:

```python
from __future__ import annotations

def f(x: "Foo" | None): ...
```

Closes #5706.


---

_Comment by @github-actions[bot] on 2023-07-13 04:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.06ms     5.0 MB/sec    1.01      8.1±0.07ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.01   1872.8±3.29µs     8.9 MB/sec    1.00   1861.2±4.66µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.6±1.36µs    14.1 MB/sec    1.00    209.3±1.33µs    14.1 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.02ms     6.3 MB/sec    1.00      4.0±0.02ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.13ms     2.9 MB/sec    1.01     14.3±0.13ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.8 MB/sec    1.01      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.7±1.00µs     6.8 MB/sec    1.00    432.9±0.57µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.08ms     4.1 MB/sec    1.00      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.05ms     6.0 MB/sec    1.03      7.0±0.05ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.4±3.53µs    11.4 MB/sec    1.03   1502.8±2.50µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.5±0.28µs    17.7 MB/sec    1.02    169.7±0.32µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.3 MB/sec    1.02      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.08ms     4.3 MB/sec    1.01      9.5±0.09ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    240.4±5.35µs    12.3 MB/sec    1.01    241.7±7.57µs    12.2 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.01      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     16.0±0.37ms     2.5 MB/sec    1.00     15.8±0.25ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   499.7±10.03µs     5.9 MB/sec    1.00    499.5±8.00µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1702.5±19.65µs     9.8 MB/sec    1.01  1713.9±24.80µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.1±2.94µs    14.8 MB/sec    1.00    199.5±2.48µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila approved on 2023-07-13 05:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:157 on 2023-07-13 06:35_

You love your nested ifs ;)

---

_@MichaReiser approved on 2023-07-13 06:35_

---

_@konstin reviewed on 2023-07-13 06:45_

---

_Review comment by @konstin on `crates/ruff_python_semantic/src/analyze/typing.rs`:157 on 2023-07-13 06:45_

and i thought i'm the one looking at SIM102 too long ;)

---

_@charliermarsh reviewed on 2023-07-13 11:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/typing.rs`:157 on 2023-07-13 11:33_

I’m sorry, it’s just how my brain works!

---

_Merged by @charliermarsh on 2023-07-13 11:34_

---

_Closed by @charliermarsh on 2023-07-13 11:34_

---

_Branch deleted on 2023-07-13 11:34_

---
