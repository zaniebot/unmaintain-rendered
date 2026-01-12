```yaml
number: 5866
title: "Implement `any_over_expr` for type alias and type params"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-any-expr
created_at: 2023-07-18T16:20:58Z
updated_at: 2023-07-19T21:17:08Z
url: https://github.com/astral-sh/ruff/pull/5866
synced_at: 2026-01-12T03:30:22Z
```

# Implement `any_over_expr` for type alias and type params

---

_Pull request opened by @zanieb on 2023-07-18 16:20_

Part of https://github.com/astral-sh/ruff/issues/5062

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/helpers.rs`:1782 on 2023-07-18 16:45_

You can also use `TextSize::default()` which may better convey the intent.

---

_@charliermarsh reviewed on 2023-07-18 16:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/helpers.rs`:1786 on 2023-07-18 16:46_

You can also use `assert!` if comparing to a boolean, but I don't feel strongly that it's "better".

---

_@charliermarsh reviewed on 2023-07-18 16:46_

---

_@charliermarsh approved on 2023-07-18 16:46_

---

_Comment by @github-actions[bot] on 2023-07-18 16:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.03ms     4.2 MB/sec    1.00      9.8±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1891.5±10.15µs     8.8 MB/sec    1.00   1893.0±3.42µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    208.3±8.56µs    14.2 MB/sec    1.00    209.0±2.25µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.00ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.00     13.7±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.02ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.9±0.90µs     7.8 MB/sec    1.00    375.9±3.06µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1452.3±5.11µs    11.5 MB/sec    1.00   1449.1±9.97µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.1±0.96µs    19.7 MB/sec    1.01    151.2±0.19µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09     11.8±0.10ms     3.4 MB/sec    1.00     10.9±0.17ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.08      2.3±0.04ms     7.1 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.03    257.8±5.53µs    11.4 MB/sec    1.00   249.6±12.28µs    11.8 MB/sec
formatter/pydantic/types.py                1.09      5.1±0.06ms     5.0 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.14ms     2.7 MB/sec    1.01     15.4±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.06ms     4.1 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    485.0±7.58µs     6.1 MB/sec    1.00    483.9±5.31µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.06ms     5.2 MB/sec    1.00      7.9±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1652.0±14.25µs    10.1 MB/sec    1.00  1650.4±28.73µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    189.6±2.88µs    15.6 MB/sec    1.00    187.8±2.75µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.05ms     7.2 MB/sec    1.00      3.5±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-07-18 16:55_

---

_@zanieb reviewed on 2023-07-18 17:03_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/helpers.rs`:1783 on 2023-07-18 17:03_

This test is a bit contrived but I'd appreciate any general Rust feedback as I'm using it for practice!

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/helpers.rs`:1783 on 2023-07-18 20:44_

I think it's fine. Or let's say, it's what the API allows you to do. You could have considered changing the `Fn` to `FnMut` so that you don't need to use `RefCell` (I guess it was a good learning opportunity ;)). 

I'm normally too lazy to bother with building the tree manually and instead parse the source code. Now, using the source code makes asserting a bit annoying because you don't have access to the node instances. That's where you could collect `NodeKind`s instead (you can get the kind by calling `AnyNodeRef::from(expr).kind()`).  However, `NodeKind` only works if you never have to distinguish between two nodes of the same kind. Here some tests that I wrote that use `NodeKind`.  https://github.com/astral-sh/ruff/blob/5d41c832ade7f5fcde5b535a8fb9784ec199489f/crates/ruff_python_ast/src/visitor/preorder.rs#L955-L1039

With an example snapshot
https://github.com/astral-sh/ruff/blob/5d41c832ade7f5fcde5b535a8fb9784ec199489f/crates/ruff_python_ast/src/visitor/snapshots/ruff_python_ast__visitor__preorder__tests__compare.snap#L1-L15


That said. I think what you have is good. The only thing I'm not a 100% sure of is if I would bother refactoring this test if I ever end up breaking it (thinking about added value / maintenance cost)

---

_@MichaReiser approved on 2023-07-18 20:44_

---

_@zanieb reviewed on 2023-07-18 21:34_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/helpers.rs`:1783 on 2023-07-18 21:34_

Thanks for taking a look!

> You could have considered changing the Fn to FnMut

It seems wrong to change the API just for testing.

> I'm not a 100% sure of is if I would bother refactoring this test if I ever end up breaking it 

Definitely. That's kind of what I was getting at with it being a bit contrived.

> I'm normally too lazy to bother with building the tree manually and instead parse the source code.

Ah that's very wise. I feel like that's much more maintainable. I'll look into doing that.


---

_@zanieb reviewed on 2023-07-19 21:17_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/helpers.rs`:1783 on 2023-07-19 21:17_

Eh this is kind of hard to do. I'd rather address it in a follow-up or next time someone wants to add a test here.

---

_Merged by @zanieb on 2023-07-19 21:17_

---

_Closed by @zanieb on 2023-07-19 21:17_

---

_Branch deleted on 2023-07-19 21:17_

---
