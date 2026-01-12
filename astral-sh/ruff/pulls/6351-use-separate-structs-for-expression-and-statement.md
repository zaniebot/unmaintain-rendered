```yaml
number: 6351
title: Use separate structs for expression and statement tracking
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/expressions-ii
created_at: 2023-08-04T18:38:18Z
updated_at: 2023-08-07T15:51:51Z
url: https://github.com/astral-sh/ruff/pull/6351
synced_at: 2026-01-12T02:52:04Z
```

# Use separate structs for expression and statement tracking

---

_Pull request opened by @charliermarsh on 2023-08-04 18:38_

## Summary

This PR fixes the performance degradation introduced in https://github.com/astral-sh/ruff/pull/6345. Instead of using the generic `Nodes` structs, we now use separate `Statement` and `Expression` structs. Importantly, we can avoid tracking a bunch of state for expressions that we need for parents: we don't need to track reference-to-ID pointers (we just have no use-case for this -- I'd actually like to remove this from statements too, but we need it for branch detection right now), we don't need to track depth, etc.

In my testing, this entirely removes the regression on all-rules, and gets us down to 2ms slower on the default rules (as a crude hyperfine benchmark, so this is within margin of error IMO).

No behavioral changes.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-04 18:38_

---

_Label `internal` added by @charliermarsh on 2023-08-04 18:44_

---

_Comment by @github-actions[bot] on 2023-08-04 19:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.1±0.44ms     4.0 MB/sec    1.00      9.8±0.30ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1846.7±96.06µs     9.0 MB/sec    1.02  1881.6±83.02µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00   216.3±11.89µs    13.6 MB/sec    1.02   220.9±15.12µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.15ms     6.6 MB/sec    1.00      3.9±0.15ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.03     13.1±0.74ms     3.1 MB/sec    1.00     12.8±0.52ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.12ms     5.1 MB/sec    1.02      3.4±0.12ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   495.3±29.62µs     6.0 MB/sec    1.00   494.9±35.73µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.3±0.29ms     4.1 MB/sec    1.00      6.1±0.37ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.06      6.8±0.28ms     6.0 MB/sec    1.00      6.4±0.25ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1418.1±65.61µs    11.7 MB/sec    1.00  1333.2±49.80µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.08    178.5±9.56µs    16.5 MB/sec    1.00    165.6±8.18µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.0±0.11ms     8.6 MB/sec    1.00      2.8±0.11ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.14ms     4.0 MB/sec    1.00     10.1±0.17ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1948.2±39.26µs     8.5 MB/sec    1.00  1922.7±23.46µs     8.7 MB/sec
formatter/numpy/globals.py                 1.01    218.2±9.50µs    13.5 MB/sec    1.00    217.1±8.95µs    13.6 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.06ms     6.0 MB/sec    1.00      4.2±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.19ms     3.1 MB/sec    1.01     13.2±0.16ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.07ms     4.7 MB/sec    1.01      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   435.4±11.61µs     6.8 MB/sec    1.00    433.6±6.36µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.10ms     4.3 MB/sec    1.02      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.09ms     5.9 MB/sec    1.02      7.0±0.09ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1423.5±20.05µs    11.7 MB/sec    1.00  1418.0±18.38µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    163.4±3.50µs    18.1 MB/sec    1.00    160.5±2.57µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.04ms     8.3 MB/sec    1.00      3.0±0.05ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/expressions.rs`:17 on 2023-08-05 08:06_

The name here will conflict with `Expr` if we go ahead and rename it to `Expression`. 

Maybe `ExpressionWithParent`. The name doesn't seem as important as it is an internal type.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/statements.rs`:33 on 2023-08-05 08:11_

Could we remove `RefEquality` if we use a `TextRange` here?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/statements.rs`:69 on 2023-08-05 08:13_

Nit: The `parent` and `parent_id` methods are asymetric in that one accepts a `StatementId` and the other `Stmt`. Are we back to `statement_parent` 😆 ? Or `resolve_parent` or require two steps: `statement_id(stmt)` and then call `parent`?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/statements.rs`:94 on 2023-08-05 08:15_

Do we need the mutability (and indexing)? Same for `Expr`.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/statements.rs`:26 on 2023-08-05 08:16_

Is this a frequent operation or could we compute the `depth` by counting the ancestor chain`?

---

_@MichaReiser approved on 2023-08-05 08:16_

---

_@charliermarsh reviewed on 2023-08-07 14:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/statements.rs`:33 on 2023-08-07 14:59_

I think so... I'll do this separately.

---

_@charliermarsh reviewed on 2023-08-07 15:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/statements.rs`:69 on 2023-08-07 15:02_

Haha. I will fix this in a follow-up since this API already exists on `Nodes` (now removed).

---

_@charliermarsh reviewed on 2023-08-07 15:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/statements.rs`:94 on 2023-08-07 15:02_

No! Good call.

---

_@charliermarsh reviewed on 2023-08-07 15:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/statements.rs`:26 on 2023-08-07 15:03_

I will revisit in a follow-up PR, since this already exists on `Nodes` (now removed).

---

_Merged by @charliermarsh on 2023-08-07 15:27_

---

_Closed by @charliermarsh on 2023-08-07 15:27_

---

_Branch deleted on 2023-08-07 15:27_

---
