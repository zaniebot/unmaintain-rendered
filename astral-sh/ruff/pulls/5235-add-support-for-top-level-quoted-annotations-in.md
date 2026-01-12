```yaml
number: 5235
title: Add support for top-level quoted annotations in RUF013
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/quoted
created_at: 2023-06-21T01:41:02Z
updated_at: 2023-06-21T14:23:38Z
url: https://github.com/astral-sh/ruff/pull/5235
synced_at: 2026-01-12T03:43:30Z
```

# Add support for top-level quoted annotations in RUF013

---

_Pull request opened by @charliermarsh on 2023-06-21 01:41_

## Summary

This PR adds support for autofixing annotations like:

```python
def f(x: "int" = None):
    ...
```

However, we don't yet support nested quotes, like:

```python
def f(x: Union["int", "str"] = None):
    ...
```

Closes #5231.


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-06-21 01:41_

---

_Comment by @github-actions[bot] on 2023-06-21 02:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      8.3±0.32ms     4.9 MB/sec    1.00      7.7±0.46ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.04  1726.1±67.96µs     9.6 MB/sec    1.00  1665.2±89.56µs    10.0 MB/sec
formatter/numpy/globals.py                 1.02   173.3±12.54µs    17.0 MB/sec    1.00   170.1±24.90µs    17.3 MB/sec
formatter/pydantic/types.py                1.08      3.5±0.18ms     7.4 MB/sec    1.00      3.2±0.20ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.55ms     2.7 MB/sec    1.04     15.8±1.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.13ms     4.4 MB/sec    1.02      3.9±0.19ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   496.8±29.09µs     5.9 MB/sec    1.01   500.2±24.08µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.31ms     3.8 MB/sec    1.00      6.7±0.30ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.13      8.7±0.50ms     4.7 MB/sec    1.00      7.7±0.34ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.12  1841.0±74.63µs     9.0 MB/sec    1.00  1644.7±65.15µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.05   216.1±11.47µs    13.7 MB/sec    1.00   205.8±14.38µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.12      3.9±0.13ms     6.5 MB/sec    1.00      3.5±0.18ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.3±0.54ms     4.4 MB/sec     1.08     10.0±0.32ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.08ms     8.3 MB/sec     1.02      2.0±0.11ms     8.1 MB/sec
formatter/numpy/globals.py                 1.01   209.8±16.12µs    14.1 MB/sec     1.00   208.5±10.81µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.17ms     6.5 MB/sec     1.06      4.2±0.21ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     18.2±1.05ms     2.2 MB/sec     1.00     18.1±0.93ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.8±0.36ms     3.4 MB/sec     1.00      4.7±0.19ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   572.7±36.32µs     5.2 MB/sec     1.00   559.7±24.74µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.0±0.35ms     3.2 MB/sec     1.00      7.9±0.43ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.49ms     4.4 MB/sec     1.05      9.7±0.69ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1997.2±143.17µs     8.3 MB/sec    1.07      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.0±16.52µs    12.3 MB/sec     1.10   263.8±16.65µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.15ms     6.2 MB/sec     1.11      4.5±0.15ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/rules/implicit_optional.rs`:344 on 2023-06-21 02:19_

nit: unused and might be confused with annotation kind down below

```suggestion
        if let Expr::Constant(ast::ExprConstant {
            range,
            value: Constant::Str(value),
            ..
        }) = annotation.as_ref()
```

---

_@dhruvmanila approved on 2023-06-21 02:28_

Thanks for doing this! I completely forgot about forward references. I think nested should be easily supported through recursion itself. If we encounter a `str`, then we parse and create a `TypingTarget` from the parsed expression if it's simple. We might want to figure out how to handle complex annotations.

---

_Comment by @charliermarsh on 2023-06-21 02:30_

Feel free to take on nested :)

"Complex" we can just ignore -- it's an edge case (implicit string concatenations), we don't fix other rules in complex annotations either.

---

_Comment by @dhruvmanila on 2023-06-21 02:56_

> Feel free to take on nested :)
> 
> "Complex" we can just ignore -- it's an edge case (implicit string concatenations), we don't fix other rules in complex annotations either.

Yeah sure, you can merge this. I'll work on it in a bit.

---

_Comment by @charliermarsh on 2023-06-21 03:09_

I'll merge in a bit, cutting another release first.

---

_Merged by @charliermarsh on 2023-06-21 14:23_

---

_Closed by @charliermarsh on 2023-06-21 14:23_

---

_Branch deleted on 2023-06-21 14:23_

---
