```yaml
number: 6257
title: "Fix formatting of `lambda` star arguments"
type: pull_request
state: merged
author: LaBatata101
labels: []
assignees: []
merged: true
base: main
head: fix-format-lambda-start-arg-bug
created_at: 2023-08-01T21:35:15Z
updated_at: 2023-08-02T20:16:28Z
url: https://github.com/astral-sh/ruff/pull/6257
synced_at: 2026-01-12T15:55:21Z
```

# Fix formatting of `lambda` star arguments

---

_@LaBatata101_

## Summary
Previously, the ruff formatter was removing the star argument of `lambda` expressions when formatting.

Given the following code snippet
```python
lambda *a: ()
lambda **b: ()
```
it would be formatted to
```python
lambda: ()
lambda: ()
```

We fix this by checking for the presence of `args`, `vararg` or `kwarg` in the `lambda` expression, before we were only checking for the presence of `args`.

Fixes #5894

## Test Plan

Add new tests cases.


---

_Comment by @LaBatata101 on 2023-08-01 21:36_

I don't know why the `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__power_op_spacing.py.snap` file got deleted. I didn't touch that file.

---

_Comment by @github-actions[bot] on 2023-08-01 22:06_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.04ms     4.9 MB/sec    1.00      8.3±0.11ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1639.0±22.78µs    10.2 MB/sec    1.00  1636.6±19.38µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    183.1±5.38µs    16.1 MB/sec    1.01    184.6±6.74µs    16.0 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.10ms     7.1 MB/sec    1.00      3.6±0.09ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.15ms     3.7 MB/sec    1.01     11.1±0.08ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    379.3±1.25µs     7.8 MB/sec    1.00    379.4±0.90µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.08ms     5.2 MB/sec    1.00      4.9±0.05ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.06ms     7.1 MB/sec    1.01      5.8±0.12ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1196.7±7.88µs    13.9 MB/sec    1.00   1194.2±2.93µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    132.5±0.56µs    22.3 MB/sec    1.00    132.0±0.20µs    22.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.04ms    10.1 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.2±0.26ms     3.3 MB/sec    1.01     12.3±0.31ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.09ms     7.0 MB/sec    1.00      2.4±0.07ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   259.8±12.06µs    11.4 MB/sec    1.03   267.6±15.16µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.13ms     4.9 MB/sec    1.02      5.3±0.16ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.01     16.9±0.35ms     2.4 MB/sec    1.00     16.7±0.31ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.10ms     3.8 MB/sec    1.02      4.4±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   544.8±46.87µs     5.4 MB/sec    1.00   532.2±15.64µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.40ms     3.4 MB/sec    1.00      7.5±0.24ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.19ms     4.6 MB/sec    1.01      9.0±0.18ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1820.0±46.74µs     9.1 MB/sec    1.01  1832.3±41.94µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.4±6.72µs    14.5 MB/sec    1.02    207.0±5.51µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.08ms     6.5 MB/sec    1.01      3.9±0.10ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-02 00:45_

Deleted is good! Deleted means exact compatibility with Black on that file.

---

_Comment by @LaBatata101 on 2023-08-02 01:03_

> Deleted is good! Deleted means exact compatibility with Black on that file.

Ah nice! Do you know why the CI is failing?

---

_@zanieb reviewed on 2023-08-02 04:05_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:79 on 2023-08-02 04:05_

Unfortunately you can do disturbing things like insert a comment between the star and the parameter. We'll probably want test coverage for cases like this:

```python
(
    lambda
    *
    # test
    x: print(x)
)
```

---

_Comment by @zanieb on 2023-08-02 04:07_

> Ah nice! Do you know why the CI is failing?

Hm I'm not sure if this panic is relevant?

> thread '<unnamed>' panicked at 'SimpleToken { kind: Star, range: 26869..26870 }', crates/ruff_python_formatter/src/other/parameters.rs:353:13

but it detected a stability error

> 2023-08-01T21:41:00.271553Z  INFO Found 1 stability errors in 2011 files (similarity index 0.759) in 86.13s

@konstin is probably best to direct you to debug that

---

_@LaBatata101 reviewed on 2023-08-02 12:45_

---

_Review comment by @LaBatata101 on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:79 on 2023-08-02 12:45_

OMG! 

---

_Comment by @charliermarsh on 2023-08-02 18:52_

I'll take a look.

---

_Comment by @konstin on 2023-08-02 18:55_

fwiw i have improving the script on my agenda for tomorrow 

---

_Comment by @charliermarsh on 2023-08-02 19:23_

Looks like it was an unrelated bug in our handling of (e.g.) `lambda *, x: 1`. It was masked before, since we didn't handle arguments at all in those cases.

---

_Merged by @charliermarsh on 2023-08-02 19:31_

---

_Closed by @charliermarsh on 2023-08-02 19:31_

---

_Branch deleted on 2023-08-02 20:16_

---
