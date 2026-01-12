```yaml
number: 5089
title: Fix a number of formatter errors from the cpython repository
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix-cpython-errors
created_at: 2023-06-14T15:22:22Z
updated_at: 2023-06-15T11:47:48Z
url: https://github.com/astral-sh/ruff/pull/5089
synced_at: 2026-01-12T15:55:17Z
```

# Fix a number of formatter errors from the cpython repository

---

_@konstin_

## Summary

This fixes a number of problems in the formatter that showed up with various files in the [cpython](https://github.com/python/cpython) repository. These problems surfaced as unstable formatting and invalid code. This is not the entirety of problems discovered through cpython, but a big enough chunk to separate it. Individual fixes are generally individual commits. They were discovered with #5055, which i update as i work through the output

## Test Plan

I added regression tests with links to cpython for each entry, except for the two stubs that also got comment stubs since they'll be implemented properly later.


---

_Label `autoformatter` added by @konstin on 2023-06-14 15:22_

---

_Review requested from @charliermarsh by @konstin on 2023-06-14 15:22_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/node.rs`:4218 on 2023-06-14 15:23_

What about match statements?

---

_@charliermarsh reviewed on 2023-06-14 15:23_

---

_@konstin reviewed on 2023-06-14 15:24_

---

_Review comment by @konstin on `crates/ruff_python_ast/src/node.rs`:4218 on 2023-06-14 15:24_

good point, thanks!

---

_Comment by @github-actions[bot] on 2023-06-14 15:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1397.2±3.32µs    11.9 MB/sec    1.01   1409.6±1.72µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    138.7±1.67µs    21.3 MB/sec    1.01    139.4±0.17µs    21.2 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.02      2.8±0.01ms     9.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.02ms     2.8 MB/sec    1.05     15.0±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.03      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.0±2.33µs     8.0 MB/sec    1.02    374.7±1.19µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.03      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.6 MB/sec    1.06      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1516.9±3.31µs    11.0 MB/sec    1.05   1588.8±4.08µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.2±0.17µs    17.9 MB/sec    1.03    170.3±0.65µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.04      3.4±0.00ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.05ms     5.1 MB/sec    1.00      7.9±0.05ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1599.7±15.27µs    10.4 MB/sec    1.01  1610.3±13.46µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    157.0±1.82µs    18.8 MB/sec    1.02    159.9±3.94µs    18.4 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     7.9 MB/sec    1.01      3.3±0.07ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.26ms     2.4 MB/sec    1.00     16.8±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.10ms     3.8 MB/sec    1.00      4.3±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   445.1±16.23µs     6.6 MB/sec    1.00    436.8±7.40µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.06ms     3.5 MB/sec    1.01      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.06ms     4.8 MB/sec    1.00      8.4±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1774.8±15.08µs     9.4 MB/sec    1.00  1755.9±17.25µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.0±1.91µs    15.6 MB/sec    1.00    188.1±2.59µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.6 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:338 on 2023-06-14 21:00_

Nit: add `python` after the three ticks?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_constant.rs`:54 on 2023-06-14 21:02_

I've long wondered if it'd be a simplification to store implicit string concatenations in the AST (like explicit concatenations). The need to re-parse strings and deal with implicit concatenations bites us everywhere.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_list.rs`:31 on 2023-06-14 21:05_

The comments are great.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_list.rs`:26 on 2023-06-14 21:05_

Same here, can we try to add `python` for consistency so as to disambiguate from actual Rust in the future?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/arguments.rs`:73 on 2023-06-14 21:11_

BTW, another place you can see some of these tricks as a sanity-check is in `Generator:

```rust
if args.vararg.is_some() || !args.kwonlyargs.is_empty() {
    self.p_delim(&mut first, ", ");
    self.p("*");
}
```

---

_@charliermarsh approved on 2023-06-14 21:11_

LGTM!

---

_@konstin reviewed on 2023-06-15 08:18_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_constant.rs`:54 on 2023-06-15 08:18_

yeah i'd prefer it if the AST would keep the string parts as separate

---

_@konstin reviewed on 2023-06-15 11:16_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_list.rs`:26 on 2023-06-15 11:16_

checked all the places with triple backticks

---

_Merged by @konstin on 2023-06-15 11:24_

---

_Closed by @konstin on 2023-06-15 11:24_

---

_Branch deleted on 2023-06-15 11:24_

---
