```yaml
number: 6795
title: Remove lexing from flake8-pytest-style
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/parameterize
created_at: 2023-08-22T21:40:35Z
updated_at: 2023-08-23T16:34:16Z
url: https://github.com/astral-sh/ruff/pull/6795
synced_at: 2026-01-12T02:45:38Z
```

# Remove lexing from flake8-pytest-style

---

_Pull request opened by @charliermarsh on 2023-08-22 21:40_

## Summary

Another drive-by change to remove unnecessary custom lexing. We just need to know the parenthesized range, so we can use... `parenthesized_range`. I've also updated `parenthesized_range` to support nested parentheses.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-22 21:40_

---

_@charliermarsh reviewed on 2023-08-22 21:41_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:48 on 2023-08-22 21:41_

@MichaReiser - Requesting review primarily for this portion (supporting nested parentheses).

---

_Comment by @github-actions[bot] on 2023-08-22 21:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.5±0.04ms    11.6 MB/sec    1.00      3.5±0.03ms    11.6 MB/sec
formatter/numpy/ctypeslib.py               1.00    707.1±7.81µs    23.6 MB/sec    1.01   711.1±15.01µs    23.4 MB/sec
formatter/numpy/globals.py                 1.00     73.6±0.30µs    40.1 MB/sec    1.00     73.5±1.46µs    40.1 MB/sec
formatter/pydantic/types.py                1.00  1416.3±10.72µs    18.0 MB/sec    1.00  1414.2±16.51µs    18.0 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.05ms     3.9 MB/sec    1.00     10.3±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.00ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    313.8±1.49µs     9.4 MB/sec    1.00    314.8±1.37µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.04ms     4.7 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.4 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1170.6±4.50µs    14.2 MB/sec    1.00   1175.1±2.27µs    14.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.8±0.37µs    24.2 MB/sec    1.01    122.6±0.26µs    24.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.9±0.18ms     8.4 MB/sec    1.01      4.9±0.24ms     8.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   974.2±37.87µs    17.1 MB/sec    1.01   983.8±42.75µs    16.9 MB/sec
formatter/numpy/globals.py                 1.02    100.1±5.33µs    29.5 MB/sec    1.00     98.3±5.02µs    30.0 MB/sec
formatter/pydantic/types.py                1.00  1963.0±77.07µs    13.0 MB/sec    1.01  1989.1±102.40µs    12.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.71ms     2.5 MB/sec    1.01     16.8±0.64ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.20ms     3.7 MB/sec    1.00      4.4±0.13ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   563.6±19.86µs     5.2 MB/sec    1.00   564.1±25.80µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.32ms     3.0 MB/sec    1.00      8.4±0.31ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.31ms     4.5 MB/sec    1.01      9.1±0.32ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1884.9±48.85µs     8.8 MB/sec    1.00  1871.1±66.63µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   231.7±10.37µs    12.7 MB/sec    1.01    233.4±7.53µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.17ms     6.2 MB/sec    1.00      4.0±0.16ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `internal` added by @charliermarsh on 2023-08-23 00:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/parenthesize.rs`:51 on 2023-08-23 06:15_

Nit: Is this the same as `left_tokenizer.zip(right_tokenizer).last().map(|(left, right)| TextRange::new(left.start(), right.end())`?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/tests/parenthesize.rs`:15 on 2023-08-23 06:18_

Snapshot tests are neat for asserting against large and complicated data structure but have the downside that the expected value isn't visible inside the test itself. It requires future us to read the test, find the right snapshot file (snapshot file names are very long), and then compare the values. This is significantly more work compared to having the expected value inline and using `assert_eq!(parenthesized, Some(TextRange::new(x, y)))` 


---

_@MichaReiser approved on 2023-08-23 06:19_

The changes look good to me but I disagree with using insta for the tests. Insta is an overkill for asserting against a `TextRange`. Please use `assert_eq` instead

---

_@charliermarsh reviewed on 2023-08-23 13:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:51 on 2023-08-23 13:37_

Yeah and I originally wrote it that way, but then I was worried it would eagerly evaluate `left` (and at least per the comments here, we wanted to avoid doing so).

---

_@charliermarsh reviewed on 2023-08-23 13:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/tests/parenthesize.rs`:15 on 2023-08-23 13:42_

Makes sense.

---

_@charliermarsh reviewed on 2023-08-23 13:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:51 on 2023-08-23 13:42_

(I'll test this definitively and maybe change it anyway -- this isn't as much of a hot path as in the formatter.)

---

_@charliermarsh reviewed on 2023-08-23 15:31_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:51 on 2023-08-23 15:31_

Ok, confirmed that `zip` _doesn't_ proceed the `left_tokenizer` if the `right_tokenizer` exhausts _and_ we invert the order from what you wrote above (i.e., `right_tokenizer.zip(left_tokenizer)`).

---

_Merged by @charliermarsh on 2023-08-23 15:54_

---

_Closed by @charliermarsh on 2023-08-23 15:54_

---

_Branch deleted on 2023-08-23 15:54_

---
