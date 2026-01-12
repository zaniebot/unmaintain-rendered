```yaml
number: 6315
title: Generalize bracketed end-of-line comment handling
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/placement
created_at: 2023-08-03T19:54:50Z
updated_at: 2023-08-04T06:02:46Z
url: https://github.com/astral-sh/ruff/pull/6315
synced_at: 2026-01-12T02:52:04Z
```

# Generalize bracketed end-of-line comment handling

---

_Pull request opened by @charliermarsh on 2023-08-03 19:54_

Micha suggested this in https://github.com/astral-sh/ruff/pull/6274#discussion_r1282774151, and it allows us to unify the implementations for arguments and type params.


---

_Review requested from @zanieb by @charliermarsh on 2023-08-03 19:54_

---

_@zanieb reviewed on 2023-08-03 20:11_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/comments/placement.rs`:50 on 2023-08-03 20:11_

Why not 
```suggestion
        AnyNodeRef::Arguments(_) | AnyNodeRef::TypeParams(_) => handle_bracketed_end_of_line_comment(comment, locator),
```

---

_@zanieb approved on 2023-08-03 20:14_

---

_Comment by @github-actions[bot] on 2023-08-03 20:31_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.45ms     4.0 MB/sec    1.03     10.3±0.30ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1954.4±64.24µs     8.5 MB/sec    1.03      2.0±0.07ms     8.3 MB/sec
formatter/numpy/globals.py                 1.03   233.3±11.86µs    12.6 MB/sec    1.00   226.2±13.33µs    13.0 MB/sec
formatter/pydantic/types.py                1.02      4.4±0.17ms     5.8 MB/sec    1.00      4.3±0.15ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.58ms     2.9 MB/sec    1.04     14.8±0.45ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.17ms     4.7 MB/sec    1.02      3.6±0.16ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   505.7±27.97µs     5.8 MB/sec    1.00   498.3±22.45µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.22ms     4.0 MB/sec    1.05      6.6±0.22ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.21ms     5.7 MB/sec    1.19      8.5±0.31ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1483.9±57.52µs    11.2 MB/sec    1.18  1747.9±45.85µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.7±6.84µs    16.9 MB/sec    1.17   204.3±10.62µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.13ms     7.9 MB/sec    1.16      3.7±0.10ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.13ms     4.1 MB/sec    1.04     10.4±0.22ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1936.9±36.52µs     8.6 MB/sec    1.03  1986.6±28.11µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00   212.0±10.63µs    13.9 MB/sec    1.03    218.6±5.83µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.15ms     6.0 MB/sec    1.02      4.3±0.07ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.03     14.0±0.27ms     2.9 MB/sec    1.00     13.6±0.31ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.7±0.08ms     4.5 MB/sec    1.00      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    446.8±8.14µs     6.6 MB/sec    1.00    444.5±6.64µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.10ms     4.0 MB/sec    1.00      6.4±0.13ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.11ms     5.5 MB/sec    1.01      7.4±0.11ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1508.4±21.89µs    11.0 MB/sec    1.00  1499.5±19.69µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.9±2.89µs    17.4 MB/sec    1.00    169.7±2.64µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.01      3.3±0.10ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @charliermarsh on 2023-08-03 20:38_

---

_Comment by @konstin on 2023-08-03 20:42_

Evil edge case:
```python
f(
    ( # a
    b
    ),
)
```
This works (it's an edge case anyway), but not as intended:
```python
f(  # a
    (b),
)
```

---

_Merged by @charliermarsh on 2023-08-03 20:51_

---

_Closed by @charliermarsh on 2023-08-03 20:51_

---

_Branch deleted on 2023-08-03 20:51_

---

_Comment by @MichaReiser on 2023-08-04 06:02_

> Evil edge case:
> 
> ```python
> f(
>     ( # a
>     b
>     ),
> )
> ```
> 
> This works (it's an edge case anyway), but not as intended:
> 
> ```python
> f(  # a
>     (b),
> )
> ```

Argh yeah, the single argument call expression. 

---
