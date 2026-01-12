```yaml
number: 6374
title: "Use dedicated AST nodes on `MemberKind`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/member-kind
created_at: 2023-08-06T14:12:08Z
updated_at: 2023-08-07T17:37:18Z
url: https://github.com/astral-sh/ruff/pull/6374
synced_at: 2026-01-12T02:52:04Z
```

# Use dedicated AST nodes on `MemberKind`

---

_Pull request opened by @charliermarsh on 2023-08-06 14:12_

## Summary

This PR leverages the unified function definition node to add precise AST node types to `MemberKind`, which is used to power our docstring definition tracking (e.g., classes and functions, whether they're methods or functions or nested functions and so on, whether they have a docstring, etc.). It was painful to do this in the past because the function variants needed to support a union anyway, but storing precise nodes removes like a dozen panics.

No behavior changes -- purely a refactor.

## Test Plan

`cargo test`


---

_Renamed from "Use dedicated AST nodes on MemberKind" to "Use dedicated AST nodes on `MemberKind`" by @charliermarsh on 2023-08-06 14:12_

---

_Label `internal` added by @charliermarsh on 2023-08-06 14:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/cast.rs`:5 on 2023-08-06 14:12_

For example, these functions and all their usages are now removed! No more `panic!` to extract these fields.

---

_@charliermarsh reviewed on 2023-08-06 14:12_

---

_Comment by @charliermarsh on 2023-08-06 14:13_

(Not gonna request any reviewers, I know this is an improvement and it's a non-behavioral change.)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/analyze/deferred_scopes.rs`:192 on 2023-08-07 07:07_

Nit: this feels more difficult to understand than what we used to have. Furthermore, the check for `shadowed.kind.is_function_definition` and the `if let Stmt::FunctionDef(...) = ..` seem redundant. Do you see a way to remove the redundancy / simplify the API?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_annotations/helpers.rs`:35 on 2023-08-07 07:09_

Nit: We have this pattern in multiple places. How about a `definition.is_function_or_method` helper function?

---

_@MichaReiser approved on 2023-08-07 07:11_

Where's the joy without any panics ;)

---

_@charliermarsh reviewed on 2023-08-07 16:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/deferred_scopes.rs`:192 on 2023-08-07 16:57_

They're actually note quite redundant, because you can have bindings whose source statement is a function definition, but that aren't _themselves_ function definitions (for example, function parameters). Trying to clean up a bit.

---

_Comment by @github-actions[bot] on 2023-08-07 17:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.32ms     4.1 MB/sec    1.02     10.0±0.68ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1902.1±71.23µs     8.8 MB/sec    1.01  1930.0±58.59µs     8.6 MB/sec
formatter/numpy/globals.py                 1.04   217.0±12.75µs    13.6 MB/sec    1.00    209.7±8.92µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.08ms     6.3 MB/sec    1.00      4.0±0.15ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.0±0.24ms     3.4 MB/sec    1.11     13.4±0.57ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.06ms     5.2 MB/sec    1.08      3.5±0.12ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   465.2±14.94µs     6.3 MB/sec    1.06   494.4±30.93µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.22ms     4.4 MB/sec    1.04      6.0±0.19ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.15ms     6.5 MB/sec    1.14      7.1±0.39ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1382.2±45.16µs    12.0 MB/sec    1.06  1464.4±37.34µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.9±6.39µs    17.5 MB/sec    1.12   188.9±26.53µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.06ms     8.9 MB/sec    1.08      3.1±0.11ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.18ms     3.9 MB/sec    1.01     10.4±0.21ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1906.4±39.27µs     8.7 MB/sec    1.03  1957.5±40.61µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    212.7±7.41µs    13.9 MB/sec    1.01    215.2±8.02µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.08ms     6.0 MB/sec    1.01      4.3±0.07ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.19ms     3.2 MB/sec    1.02     13.2±0.19ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.08ms     4.7 MB/sec    1.01      3.6±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.0±5.88µs     6.9 MB/sec    1.02   435.5±19.39µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.10ms     4.3 MB/sec    1.02      6.1±0.16ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.07ms     5.9 MB/sec    1.01      6.9±0.08ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1430.1±26.93µs    11.6 MB/sec    1.01  1447.8±26.81µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.8±4.05µs    18.1 MB/sec    1.02    166.2±5.35µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.05ms     8.3 MB/sec    1.03      3.1±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-07 17:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/helpers.rs`:35 on 2023-08-07 17:08_

Fixed, good call.

---

_Merged by @charliermarsh on 2023-08-07 17:17_

---

_Closed by @charliermarsh on 2023-08-07 17:17_

---

_Branch deleted on 2023-08-07 17:18_

---
