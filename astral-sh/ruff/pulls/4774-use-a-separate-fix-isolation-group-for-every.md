```yaml
number: 4774
title: Use a separate fix-isolation group for every parent node
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isolation
created_at: 2023-06-01T03:10:18Z
updated_at: 2023-06-02T03:29:38Z
url: https://github.com/astral-sh/ruff/pull/4774
synced_at: 2026-01-12T03:50:03Z
```

# Use a separate fix-isolation group for every parent node

---

_Pull request opened by @charliermarsh on 2023-06-01 03:10_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is a follow-up to #4766 aimed at improving performance of deletions. In #4766, we removed our stateful deletion-tracking code, and replaced it with a "fix isolation" concept, whereby we only apply one "isolated" fix per Ruff execution.

We can actually do quite a bit better by modifying this concept a bit: instead of using one isolation group per file, we can create an isolation group for every parent. This allows us to fix both of these unused imports in the same pass:

```py
if True:
  import os

if True:
  import sys
```

## Test Plan

We can benchmark the fix time on CPython via:

```shell
cargo build --release && hyperfine --ignore-failure --warmup 10 --runs 50 "./target/release/ruff ./crates/ruff/resources/test/cpython/ --select F --no-cache --silent --fix" --prepare "cd ./crates/ruff/resources/test/cpython/ && git reset --hard HEAD"
```

(We can either use `--select F` or `--select F401` -- by far the most impacted rule here is unused import removal.)

On `main`, `--select F` takes about 409ms, while `--select F401` takes about 266ms.

On this branch, `--select F` takes about 420ms, while `--select F401` takes about 270ms.

So there is a performance regression, but I think it's closer to the realm of reasonableness, especially for a behavior that only affects codebases with large numbers of nested diagnostics. (On #4766, `--select F` was more like ~600ms.)


---

_Comment by @github-actions[bot] on 2023-06-01 03:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.04ms     2.7 MB/sec    1.00     14.9±0.12ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.7±0.97µs     8.0 MB/sec    1.00    371.1±0.83µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.03ms     5.5 MB/sec    1.00      7.4±0.04ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1586.6±4.28µs    10.5 MB/sec    1.00   1588.4±4.15µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.6±0.68µs    16.9 MB/sec    1.00    175.1±1.66µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.0 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1141.5±2.71µs    14.6 MB/sec    1.00   1138.4±2.48µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    117.3±0.40µs    25.2 MB/sec    1.01    118.0±0.82µs    25.0 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.28ms     2.4 MB/sec    1.02     17.0±0.31ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     3.9 MB/sec    1.00      4.2±0.11ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.8±6.68µs     5.9 MB/sec    1.02   512.1±25.35µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.17ms     3.6 MB/sec    1.00      7.0±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.12ms     4.9 MB/sec    1.00      8.3±0.13ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.9±35.08µs     9.4 MB/sec    1.00  1766.2±39.10µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.2±8.84µs    14.3 MB/sec    1.00    206.2±8.10µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.09ms     6.7 MB/sec    1.00      3.8±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.5±0.11ms     6.2 MB/sec    1.00      6.5±0.12ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1233.2±31.09µs    13.5 MB/sec    1.00  1218.3±39.25µs    13.7 MB/sec
parser/numpy/globals.py                    1.02    127.8±5.41µs    23.1 MB/sec    1.00    125.1±4.01µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.2 MB/sec    1.00      2.8±0.09ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:71 on 2023-06-01 06:44_

The comment is now outdated

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:35 on 2023-06-01 06:46_

How do we make sure that the group ids remain unique? I'm worried that this becomes a `z-index` like problem where people start guessing numbers. 

 I think we could either restrict the type to `NodeId` if the idea is to isolate operations on a per node or we could consider using a `TypeId` for fixed groups (probably less useful)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:173 on 2023-06-01 06:48_

Could you explain this in more detail? Should I use this for all fixes or just some fixes that apply to a certain parent? It may also be worth renaming the method to make it more explicit, what it's use case is (when to use it)

---

_@MichaReiser approved on 2023-06-01 06:49_

Clever!

My main concern is about using a `u32` in the `Isolation` enum. I think we should be more restrictive and limit it to `NodeId`s only. 

---

_@charliermarsh reviewed on 2023-06-02 02:46_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:35 on 2023-06-02 02:46_

The problem is, this crate doesn't depend on `ruff_python_semantic` :(

---

_@charliermarsh reviewed on 2023-06-02 02:58_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:35 on 2023-06-02 02:58_

I agree with you, but I can't figure out how to fix this.

---

_@charliermarsh reviewed on 2023-06-02 02:59_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:35 on 2023-06-02 02:59_

I'm going to bias towards merging, but if you have suggestions I'm more than happy to act on them.

---

_Merged by @charliermarsh on 2023-06-02 03:07_

---

_Closed by @charliermarsh on 2023-06-02 03:07_

---

_Branch deleted on 2023-06-02 03:07_

---
