```yaml
number: 5842
title: Add basic support for PEP 695
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: deps/parser
created_at: 2023-07-17T21:38:41Z
updated_at: 2023-07-18T17:49:32Z
url: https://github.com/astral-sh/ruff/pull/5842
synced_at: 2026-01-12T15:55:19Z
```

# Add basic support for PEP 695

---

_@zanieb_

Part of https://github.com/astral-sh/ruff/issues/5062

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds basic boilerplate support for PEP 695 without the expectation that we will actually _do_ anything with the contents of such expressions.

Bumps our RustPython/Parser fork to get support for PEP 695 as tracked by https://github.com/RustPython/Parser/issues/82

## Test Plan

<!-- How was it tested? -->


---

_@zanieb reviewed on 2023-07-17 21:40_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/visitor.rs`:575 on 2023-07-17 21:40_

Should we remove all visiting of constants? As far as I can tell https://github.com/astral-sh/ruff/blob/22bcd0afb428a4ef4a9426ae6374693aa9bf8043/crates/ruff_python_ast/src/visitor.rs#L510 is unused now

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/visitor.rs`:575 on 2023-07-17 22:26_

See #5840

---

_@zanieb reviewed on 2023-07-17 22:26_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/source_code/generator.rs`:553 on 2023-07-17 22:54_

Need to get a little more sophisticated in handling trailing comma, I presume.

---

_@zanieb reviewed on 2023-07-17 22:54_

---

_@zanieb reviewed on 2023-07-17 22:55_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/helpers.rs`:278 on 2023-07-17 22:55_

Should `any_over_expr` visit names?

---

_Comment by @github-actions[bot] on 2023-07-18 00:37_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.06ms     4.3 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1856.7±3.01µs     9.0 MB/sec    1.00   1850.6±3.45µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    205.0±0.44µs    14.4 MB/sec    1.01    206.7±0.51µs    14.3 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.07ms     3.1 MB/sec    1.02     13.6±0.25ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    440.4±1.37µs     6.7 MB/sec    1.00    440.4±0.48µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1418.4±6.07µs    11.7 MB/sec    1.00   1409.0±1.33µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    157.9±0.93µs    18.7 MB/sec    1.00    155.6±0.24µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.3±0.13ms     3.6 MB/sec    1.00     11.2±0.12ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.2±0.04ms     7.7 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    230.8±4.07µs    12.8 MB/sec    1.01    232.5±5.39µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.08ms     5.3 MB/sec    1.00      4.8±0.08ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.19ms     2.6 MB/sec    1.00     15.8±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.1±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.7±8.37µs     7.0 MB/sec    1.01   425.3±10.23µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.14ms     3.6 MB/sec    1.00      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.14ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1647.6±25.82µs    10.1 MB/sec    1.00  1625.3±31.98µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    172.8±3.88µs    17.1 MB/sec    1.00    170.8±2.01µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.10ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/generator.rs`:553 on 2023-07-18 02:03_

Check out the `self.p_delim(&mut first, ", ");` line in the `Stmt::With` case.

---

_@charliermarsh reviewed on 2023-07-18 02:03_

---

_@charliermarsh reviewed on 2023-07-18 02:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/helpers.rs`:278 on 2023-07-18 02:03_

It should visit all expressions, so needs to recurse on anything that might contain an expression.

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/helpers.rs`:278 on 2023-07-18 04:08_

The names can't contain expressions, they're just containers for a string. It seems like a pain if you _were_ trying to visit parameter names here but I guess we can tackle that with another helper (or as it comes up).

---

_@zanieb reviewed on 2023-07-18 04:08_

---

_Comment by @zanieb on 2023-07-18 17:49_

Superseded by #5459 

---

_Closed by @zanieb on 2023-07-18 17:49_

---
