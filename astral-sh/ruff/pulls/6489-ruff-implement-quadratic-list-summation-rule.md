```yaml
number: 6489
title: "[`ruff`] Implement `quadratic-list-summation` rule (`RUF017`)"
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - rule
assignees: []
merged: true
base: main
head: evanrittenhouse_5073
created_at: 2023-08-10T23:43:23Z
updated_at: 2023-08-18T00:00:08Z
url: https://github.com/astral-sh/ruff/pull/6489
synced_at: 2026-01-12T15:55:21Z
```

# [`ruff`] Implement `quadratic-list-summation` rule (`RUF017`)

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds `RUF017`. Closes #5073 

## Test Plan

`cargo t`


---

_Comment by @github-actions[bot] on 2023-08-11 00:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      3.6±0.30ms    11.4 MB/sec     1.00      3.5±0.30ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   710.0±47.57µs    23.5 MB/sec     1.08  770.1±110.80µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     73.1±5.37µs    40.3 MB/sec     1.11    81.2±10.64µs    36.3 MB/sec
formatter/pydantic/types.py                1.00  1431.4±109.79µs    17.8 MB/sec    1.03  1471.6±129.15µs    17.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.0±0.59ms     3.4 MB/sec     1.02     12.2±0.48ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.19ms     5.1 MB/sec     1.01      3.3±0.17ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   460.6±31.75µs     6.4 MB/sec     1.00   455.2±33.89µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.3±0.34ms     4.0 MB/sec     1.00      6.0±0.36ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      6.3±0.32ms     6.4 MB/sec     1.00      6.2±0.33ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1356.0±83.04µs    12.3 MB/sec     1.00  1332.0±67.40µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.08   174.8±11.03µs    16.9 MB/sec     1.00   162.2±10.40µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.8±0.14ms     9.2 MB/sec     1.00      2.7±0.15ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.9±0.49ms     8.3 MB/sec    1.00      4.8±0.27ms     8.5 MB/sec
formatter/numpy/ctypeslib.py               1.04   904.5±91.24µs    18.4 MB/sec    1.00   868.4±42.46µs    19.2 MB/sec
formatter/numpy/globals.py                 1.00     86.5±3.53µs    34.1 MB/sec    1.02     87.8±3.40µs    33.6 MB/sec
formatter/pydantic/types.py                1.00  1761.7±79.80µs    14.5 MB/sec    1.01  1784.7±65.37µs    14.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.8±0.63ms     2.6 MB/sec    1.00     15.7±0.63ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.19ms     3.9 MB/sec    1.00      4.2±0.20ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   488.8±20.03µs     6.0 MB/sec    1.02   497.3±36.49µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.32ms     3.1 MB/sec    1.00      8.1±0.38ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.28ms     4.8 MB/sec    1.00      8.4±0.26ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1731.8±98.74µs     9.6 MB/sec    1.00  1708.3±78.62µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03   200.9±13.71µs    14.7 MB/sec    1.00   194.5±13.12µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.14ms     6.8 MB/sec    1.00      3.6±0.12ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/quadratic_list_summation.rs`:9 on 2023-08-11 09:55_

sentence incomplete

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/quadratic_list_summation.rs`:12 on 2023-08-11 09:56_

This should explain what quadratic means and why it's slow

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/quadratic_list_summation.rs`:62 on 2023-08-11 10:02_

i would merge this match with the match from `verify_start_arg`, either by having `verify_start_arg` return this value or by inlining the function

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/quadratic_list_summation.rs`:58 on 2023-08-11 10:04_

nit: use the following pattern for slightly more straightforward code
```suggestion
    let Some(first) = args.first() else {
        return;
    }

    match first {
        Expr::Name(ast::ExprName { id, .. }) => ...,
        Expr::List(ast::ExprList { range, .. }) => ...,
        _ => return,
    }
```

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:8 on 2023-08-11 10:07_

can you add a case where the expression is multiline? your code should work but the case should be in the tests

---

_@konstin reviewed on 2023-08-11 10:08_

---

_@evanrittenhouse reviewed on 2023-08-11 13:21_

---

_Review comment by @evanrittenhouse on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:8 on 2023-08-11 13:21_

Added

---

_@konstin reviewed on 2023-08-11 13:49_

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:8 on 2023-08-11 13:49_

Is it intentional that you didn't push the change yet?

---

_Review requested from @charliermarsh by @konstin on 2023-08-11 15:20_

---

_Review comment by @evanrittenhouse on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:8 on 2023-08-11 17:10_

Should be right below this line

---

_@evanrittenhouse reviewed on 2023-08-11 17:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-16 13:20_

---

_Renamed from "Initial implementation for RUF017" to "[`ruff`] Implement `quadratic-list-summation` rule (`RUF017`)" by @charliermarsh on 2023-08-17 02:00_

---

_Label `rule` added by @charliermarsh on 2023-08-17 02:00_

---

_Comment by @charliermarsh on 2023-08-17 02:00_

Thanks! I tweaked this to use the `functools.reduce` approach suggested in the issue, since we _can_ import symbols and I find it to be more readable (closer to `sum()`). I also put this in the nursery for now, I'd like to get feedback on it before we upgrade it to production.

---

_Comment by @charliermarsh on 2023-08-17 02:01_

Oh, I'm not able to push my edits. Do you mind enabling edits, @evanrittenhouse?

---

_Comment by @evanrittenhouse on 2023-08-17 02:44_

Sounds good - enabled edits. Didn't know we are down for importing other modules - assuming that those are built-ins only, right? Are there any contexts where we wouldn't want to do that?

---

_Comment by @charliermarsh on 2023-08-17 02:57_

@evanrittenhouse - Good question. We can import anything from the standard library (`functools` and `operator` are both standard-library modules), but we'd never want to import anything outside of the standard library.

---

_Merged by @charliermarsh on 2023-08-17 03:13_

---

_Closed by @charliermarsh on 2023-08-17 03:13_

---

_@hauntsaninja reviewed on 2023-08-17 21:35_

---

_Review comment by @hauntsaninja on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF017_RUF017.py.snap`:33 on 2023-08-17 21:35_

Is it intentional this only complains when start is passed by kwarg? If so, what's the reasoning?

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF017_RUF017.py.snap`:33 on 2023-08-17 21:39_

@charliermarsh I think this is from [e38e8c0](https://github.com/astral-sh/ruff/pull/6489/commits/e38e8c0a51930e4bb8ea0d17026d1ed9b8287b71). Any reason we switched from `arguments.find_argument("start", 1)` to `arguments.find_keyword("start")`?

---

_@evanrittenhouse reviewed on 2023-08-17 21:39_

---

_@charliermarsh reviewed on 2023-08-17 23:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF017_RUF017.py.snap`:33 on 2023-08-17 23:18_

Just my mistake, @evanrittenhouse had it right initially. I looked at the docs and saw the `/` for the positional-only `iterable` argument, and my mind immediately went to keyword-only, wasn't thinking straight. PR welcome, or I'll fix a little later.

---

_@hauntsaninja reviewed on 2023-08-17 23:56_

---

_Review comment by @hauntsaninja on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF017_RUF017.py.snap`:33 on 2023-08-17 23:56_

Thanks both! Easy fix, will open a PR

---

_@hauntsaninja reviewed on 2023-08-18 00:00_

---

_Review comment by @hauntsaninja on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF017_RUF017.py.snap`:33 on 2023-08-18 00:00_

https://github.com/astral-sh/ruff/pull/6664

---
