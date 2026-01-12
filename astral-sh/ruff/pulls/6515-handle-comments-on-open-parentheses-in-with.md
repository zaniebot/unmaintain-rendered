```yaml
number: 6515
title: Handle comments on open parentheses in with statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/parenthesized_with
created_at: 2023-08-12T00:26:10Z
updated_at: 2023-08-14T15:30:46Z
url: https://github.com/astral-sh/ruff/pull/6515
synced_at: 2026-01-12T02:52:04Z
```

# Handle comments on open parentheses in with statements

---

_Pull request opened by @charliermarsh on 2023-08-12 00:26_

## Summary

This PR adds handling for comments on open parentheses in parenthesized context managers. For example, given:

```python
with (  # comment
    CtxManager1() as example1,
    CtxManager2() as example2,
    CtxManager3() as example3,
):
    ...
```

We want to preserve that formatting. (Black does the same.) On `main`, we format as:

```python
with (
    # comment
    CtxManager1() as example1,
    CtxManager2() as example2,
    CtxManager3() as example3,
):
    ...
```

It's very similar to how `StmtImportFrom` is handled.

Note that this case _isn't_ covered by the "parenthesized comment" proposal, since this is a common on the statement that would typically be attached to the first `WithItem`, and the `WithItem` _itself_ can have parenthesized comments, like:

```python
with (  # comment
    (
        CtxManager1()  # comment
    ) as example1,
    CtxManager2() as example2,
    CtxManager3() as example3,
):
    ...
```

## Test Plan

`cargo test`

Confirmed no change in similarity score.


---

_Label `formatter` added by @charliermarsh on 2023-08-12 00:26_

---

_@charliermarsh reviewed on 2023-08-12 00:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1248 on 2023-08-12 00:27_

We could generalize this (the method is nearly identical to the `ImportFrom` example), but in my opinion it's fine to just duplicate it.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_import_from.rs`:67 on 2023-08-12 00:28_

I'm also changing the `ImportFrom` handling here to preserve:

```python
from example import (  # comment
    A,
    B,
)
```

Previously, we flattened this.

---

_@charliermarsh reviewed on 2023-08-12 00:28_

---

_Comment by @github-actions[bot] on 2023-08-12 01:06_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.5±0.38ms     9.0 MB/sec     1.00      4.5±0.16ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   827.2±90.35µs    20.1 MB/sec     1.12   926.3±32.90µs    18.0 MB/sec
formatter/numpy/globals.py                 1.00     87.8±6.18µs    33.6 MB/sec     1.04     90.9±5.17µs    32.5 MB/sec
formatter/pydantic/types.py                1.00  1619.6±100.67µs    15.7 MB/sec    1.14  1843.0±70.32µs    13.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.80ms     3.0 MB/sec     1.02     13.9±0.51ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.18ms     4.9 MB/sec     1.05      3.5±0.14ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   520.4±27.13µs     5.7 MB/sec     1.00   519.2±21.66µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.38ms     3.6 MB/sec     1.03      7.2±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.35ms     5.7 MB/sec     1.08      7.6±0.17ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1461.3±90.02µs    11.4 MB/sec     1.11  1616.3±53.58µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   194.3±10.49µs    15.2 MB/sec     1.00    193.5±5.80µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.18ms     7.9 MB/sec     1.04      3.4±0.10ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.2±0.12ms     9.6 MB/sec    1.00      4.3±0.11ms     9.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   816.4±10.72µs    20.4 MB/sec    1.00   816.6±12.04µs    20.4 MB/sec
formatter/numpy/globals.py                 1.00     82.8±2.59µs    35.6 MB/sec    1.00     83.2±2.18µs    35.5 MB/sec
formatter/pydantic/types.py                1.00  1683.1±43.50µs    15.2 MB/sec    1.01  1692.1±37.21µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.11ms     3.2 MB/sec    1.01     12.9±0.16ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.01      3.5±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   441.2±12.37µs     6.7 MB/sec    1.00    437.9±5.96µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.08ms     3.9 MB/sec    1.00      6.6±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.07ms     5.8 MB/sec    1.00      6.9±0.07ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1505.2±29.36µs    11.1 MB/sec    1.00  1469.2±24.26µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.6±3.02µs    16.9 MB/sec    1.00    174.4±3.15µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.04ms     8.1 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1231 on 2023-08-14 12:12_

Nit: Name this `with_statement`. The `with_` gives me the impression that something is missing (also hard to see the `.` right after the underscore). 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:55 on 2023-08-14 12:19_

Does this account for the trailing `,` or should we use `f.join_comma_separated` here, the dangling comments formatting ensures that it renders the group in expanded mode.

---

_@MichaReiser approved on 2023-08-14 12:20_

---

_Comment by @MichaReiser on 2023-08-14 12:20_

You're bold to make changes to the with item parenthesizing logic :sweat_smile: 

---

_Comment by @charliermarsh on 2023-08-14 13:23_

> You're bold to make changes to the with item parenthesizing logic

I'm getting stronger at formatter...

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:55 on 2023-08-14 15:02_

Ahh good call, it didn't account for that.

---

_@charliermarsh reviewed on 2023-08-14 15:02_

---

_Merged by @charliermarsh on 2023-08-14 15:11_

---

_Closed by @charliermarsh on 2023-08-14 15:11_

---

_Branch deleted on 2023-08-14 15:11_

---
