```yaml
number: 6381
title: Fixup comment handling on opening parenthesis in function definition
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/inner-parenthesis-function
created_at: 2023-08-07T02:17:36Z
updated_at: 2023-08-07T20:03:46Z
url: https://github.com/astral-sh/ruff/pull/6381
synced_at: 2026-01-12T02:52:04Z
```

# Fixup comment handling on opening parenthesis in function definition

---

_Pull request opened by @charliermarsh on 2023-08-07 02:17_

## Summary

I noticed some deviations in how we treat dangling comments that hug the opening parenthesis for function definitions.

For example, given:

```python
def f(  # first
    # second
):  # third
    ...
```

We currently format as:

```python
def f(
      # first
    # second
):  # third
    ...
```

This PR adds the proper opening-parenthesis dangling comment handling for function parameters. Specifically, as with all other parenthesized nodes, we now detect that dangling comment in `placement.rs` and handle it in `parameters.rs`. We have to take some care in that file, since we have multiple "kinds" of dangling comments, but I added a bunch of test cases that we now format identically to Black.

## Test Plan

`cargo test`

Before:

- `zulip`: 0.99388
- `django`: 0.99784
- `warehouse`: 0.99504
- `transformers`: 0.99404
- `cpython`: 0.75913
- `typeshed`: 0.74364

After:

- `zulip`: 0.99386
- `django`: 0.99784
- `warehouse`: 0.99504
- `transformers`: 0.99404
- `cpython`: 0.75913
- `typeshed`: 0.74409

Meaningful improvement on `typeshed`, minor decrease on `zulip`.


---

_Review requested from @konstin by @charliermarsh on 2023-08-07 02:17_

---

_Label `formatter` added by @charliermarsh on 2023-08-07 02:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:259 on 2023-08-07 02:21_

Now using the same utilities as all the other nodes (`parenthesized_with_dangling_comments`, `empty_parenthesized_with_dangling_comments`).

---

_@charliermarsh reviewed on 2023-08-07 02:21_

---

_Comment by @github-actions[bot] on 2023-08-07 03:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     10.1±0.14ms     4.0 MB/sec    1.00      9.6±0.10ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.02  1936.5±39.72µs     8.6 MB/sec    1.00   1905.4±8.72µs     8.7 MB/sec
formatter/numpy/globals.py                 1.03    218.2±4.03µs    13.5 MB/sec    1.00    212.3±2.92µs    13.9 MB/sec
formatter/pydantic/types.py                1.03      4.2±0.09ms     6.1 MB/sec    1.00      4.1±0.03ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.09ms     3.4 MB/sec    1.01     12.2±0.14ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.06ms     5.3 MB/sec    1.03      3.2±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.5±5.44µs     6.6 MB/sec    1.01    450.6±5.71µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.05ms     4.6 MB/sec    1.00      5.5±0.09ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.06ms     6.5 MB/sec    1.02      6.4±0.05ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1345.4±10.46µs    12.4 MB/sec    1.01   1359.2±9.91µs    12.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.6±2.59µs    19.5 MB/sec    1.01    153.8±2.69µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.02ms     9.1 MB/sec    1.01      2.8±0.03ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.17ms     4.0 MB/sec    1.00     10.3±0.22ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1932.0±28.55µs     8.6 MB/sec    1.00  1923.8±45.98µs     8.7 MB/sec
formatter/numpy/globals.py                 1.01    215.3±6.71µs    13.7 MB/sec    1.00    214.2±8.63µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.15ms     5.9 MB/sec    1.00      4.3±0.08ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.4±0.27ms     3.0 MB/sec    1.00     13.3±0.32ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.08ms     4.6 MB/sec    1.00      3.6±0.10ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.03    440.1±7.47µs     6.7 MB/sec    1.00    427.7±6.90µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.15ms     4.2 MB/sec    1.00      6.0±0.15ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.12ms     5.7 MB/sec    1.00      7.0±0.16ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1440.2±22.81µs    11.6 MB/sec    1.00  1438.4±24.41µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    166.7±3.81µs    17.7 MB/sec    1.00    163.6±2.82µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.06ms     8.2 MB/sec    1.02      3.1±0.05ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:92 on 2023-08-07 07:41_

Unrelated to this PR but just popped to my head (may be relevant for other places where we apply the same logic). What happens if we have: 

```Python
call(( a # test
)) 
```

I would expect this to be a comment of `a` and not a dangling arguments comment.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:324 on 2023-08-07 07:45_

Can you add a test with a trailing `(` comment and a slash argument?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:122 on 2023-08-07 07:46_

Should we use `dangling[parenthesis_comments_end..]` here to aovid that a comment incorrectly gets formatted twice?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:77 on 2023-08-07 07:48_

The `/` can never be the first argument. Would it be safe to test whether the comment is an `end_of_line` comment and starts before the first argument?

---

_@MichaReiser reviewed on 2023-08-07 07:48_

---

_Comment by @MichaReiser on 2023-08-07 07:54_

Please update your test plan with the similarity index.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/parameters.rs`:76 on 2023-08-07 08:11_

it would be good to link https://github.com/astral-sh/ruff/issues/5247 here

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/parameters.rs`:77 on 2023-08-07 08:16_

you'd also have to check that the comment starts before the star if any because
```python
def f(
    *, # comment
    b
): pass
```
is allowed

---

_@konstin approved on 2023-08-07 08:18_

---

_Comment by @konstin on 2023-08-07 11:18_

This fixes an instability with zulip, can you add
```python
x = () - a(#
b)
```
to the tests?

---

_@charliermarsh reviewed on 2023-08-07 14:08_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:77 on 2023-08-07 14:08_

Yeah I originally wanted to do this, but it started to feel even more complicated because of the star and arguments cases.

---

_Comment by @charliermarsh on 2023-08-07 17:35_

@konstin - That's a call though, this only touches function definitions, is it related?

---

_Merged by @charliermarsh on 2023-08-07 18:04_

---

_Closed by @charliermarsh on 2023-08-07 18:04_

---

_Branch deleted on 2023-08-07 18:04_

---

_Comment by @konstin on 2023-08-07 20:03_

sorry, i got confused

---
