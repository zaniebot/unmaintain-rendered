```yaml
number: 4645
title: "Implement PYI048 for `flake8-pyi` plugin"
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: feature/addPYI048
created_at: 2023-05-24T20:38:31Z
updated_at: 2023-05-26T18:56:30Z
url: https://github.com/astral-sh/ruff/pull/4645
synced_at: 2026-01-12T15:55:16Z
```

# Implement PYI048 for `flake8-pyi` plugin

---

_@qdegraaf_

## Summary
Adds [PYI048](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/pyi.py#LL2107C7-L2107C7) rule to `flake8-pyi`.

Open question is how to flag this. Rule currently triggers a violation for every offending line. Which clearly communicates the relevant lines but could lead to a lot of clutter for a large multi-line func. Alternatives considered were highlighting the function name or the last line of a given function, both felt a little unclear, if concise. Any input on this is welcome

## Test Plan

Test files (both stub and non-stub) added. Scenarios with and without dosctrings considered to not overlap with `PYI021`: `DocstringInStub` as done in other rules and confirmed in: https://github.com/charliermarsh/ruff/pull/4634#issuecomment-1561419718 


---

_Comment by @charliermarsh on 2023-05-24 20:47_

> Open question is how to flag this. Rule currently triggers a violation for every offending line. Which clearly communicates the relevant lines but could lead to a lot of clutter for a large multi-line func. Alternatives considered were highlighting the function name or the last line of a given function, both felt a little unclear, if concise. Any input on this is welcome

I'd vote to use the name of the function. We have a utility for this (`helpers::identifier_range(stmt, self.locator)`), which will "do the right thing" if you pass in the function statement.


---

_Comment by @github-actions[bot] on 2023-05-24 21:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     19.6±0.68ms     2.1 MB/sec    1.00     19.0±0.58ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.00      4.5±0.16ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.5±22.91µs     5.1 MB/sec    1.01   582.3±18.82µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.26ms     3.2 MB/sec    1.00      7.9±0.27ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.19ms     4.6 MB/sec    1.01      9.0±0.31ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1947.0±55.57µs     8.6 MB/sec    1.01  1968.1±77.62µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    226.1±9.37µs    13.0 MB/sec    1.00   223.7±12.07µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.20ms     6.3 MB/sec    1.00      4.0±0.12ms     6.4 MB/sec
parser/large/dataset.py                    1.01      7.0±0.21ms     5.8 MB/sec    1.00      6.9±0.29ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1369.2±48.75µs    12.2 MB/sec    1.02  1395.9±52.55µs    11.9 MB/sec
parser/numpy/globals.py                    1.02    139.1±4.45µs    21.2 MB/sec    1.00    137.0±5.22µs    21.5 MB/sec
parser/pydantic/types.py                   1.04      3.1±0.07ms     8.3 MB/sec    1.00      2.9±0.10ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.06     25.7±0.65ms  1623.9 KB/sec     1.00     24.3±0.94ms  1717.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      6.3±0.23ms     2.7 MB/sec     1.00      6.1±0.45ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   723.4±38.17µs     4.1 MB/sec     1.00   708.4±46.75µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.05     10.6±0.40ms     2.4 MB/sec     1.00     10.2±0.47ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.04     12.4±0.45ms     3.3 MB/sec     1.00     12.0±0.41ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.6±0.12ms     6.4 MB/sec     1.00      2.5±0.13ms     6.6 MB/sec
linter/default-rules/numpy/globals.py      1.02   307.0±15.96µs     9.6 MB/sec     1.00   302.4±17.16µs     9.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      5.5±0.21ms     4.6 MB/sec     1.00      5.4±0.22ms     4.7 MB/sec
parser/large/dataset.py                    1.04      9.9±0.35ms     4.1 MB/sec     1.00      9.6±0.39ms     4.3 MB/sec
parser/numpy/ctypeslib.py                  1.06  1892.1±117.12µs     8.8 MB/sec    1.00  1789.6±70.68µs     9.3 MB/sec
parser/numpy/globals.py                    1.00    187.5±9.87µs    15.7 MB/sec     1.00   187.2±10.07µs    15.8 MB/sec
parser/pydantic/types.py                   1.00      4.1±0.16ms     6.2 MB/sec     1.01      4.1±0.24ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/stub_body_multiple_statements.rs`:44 on 2023-05-25 00:38_

Why include `Ellipsis` here?

---

_@charliermarsh reviewed on 2023-05-25 00:38_

---

_@charliermarsh reviewed on 2023-05-25 00:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/stub_body_multiple_statements.rs`:24 on 2023-05-25 00:39_

We have an existing `is_docstring_stmt` helper in the `ruff_python_ast` crate -- can we use that here?

---

_@qdegraaf reviewed on 2023-05-25 08:11_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/stub_body_multiple_statements.rs`:44 on 2023-05-25 08:11_

Misread logic and found something similar in B018, then forgot to double-check. I wasn't at my sharpest yesterday 😅 :
```rust
    // Ignore strings, to avoid false positives with docstrings.
    if matches!(
        value,
        Expr::JoinedStr(_)
            | Expr::Constant(ast::ExprConstant {
                value: Constant::Str(..) | Constant::Ellipsis,
                ..
            })
    ) {
        return;
    }
```

It's moot in this case as I'll use the existing helper (thanks for pointing me towards that)

---

_Merged by @charliermarsh on 2023-05-25 20:04_

---

_Closed by @charliermarsh on 2023-05-25 20:04_

---

_Branch deleted on 2023-05-26 18:56_

---
