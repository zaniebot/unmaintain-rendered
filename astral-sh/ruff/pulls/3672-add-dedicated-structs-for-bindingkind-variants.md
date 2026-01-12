```yaml
number: 3672
title: "Add dedicated structs for `BindingKind` variants"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/structs
created_at: 2023-03-22T19:00:47Z
updated_at: 2023-03-23T06:06:25Z
url: https://github.com/astral-sh/ruff/pull/3672
synced_at: 2026-01-12T04:39:45Z
```

# Add dedicated structs for `BindingKind` variants

---

_Pull request opened by @charliermarsh on 2023-03-22 19:00_

## Summary

This is a non-behavior-changing refactor to move the `BindingKind` variants to use dedicated structs.

## Test Plan

Verify that all automated checks pass as expected.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-22 19:00_

---

_Review requested from @konstin by @charliermarsh on 2023-03-22 19:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/scope.rs`:433 on 2023-03-22 19:01_

I think we can improve some of these abstractions (better names, shared traits). This PR doesn't attempt to improve the object hierarchy at all -- it's just a "straight" migration.

---

_@charliermarsh reviewed on 2023-03-22 19:01_

---

_@charliermarsh reviewed on 2023-03-22 19:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/scope.rs`:453 on 2023-03-22 19:02_

Learning opportunity for me: when would we want to `Box` these?

---

_@JonathanPlasse reviewed on 2023-03-22 19:37_

---

_Review comment by @JonathanPlasse on `crates/ruff_python_ast/src/scope.rs`:453 on 2023-03-22 19:37_

When we want the enum to have the size of a box instead of the biggest struct, I think.

---

_Comment by @github-actions[bot] on 2023-03-22 19:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.8±0.07ms     2.9 MB/sec    1.00     13.6±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.1±0.98µs     6.0 MB/sec    1.01    495.1±1.76µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.03ms     4.2 MB/sec    1.00      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      7.3±0.02ms     5.5 MB/sec    1.00      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1638.6±2.45µs    10.2 MB/sec    1.00   1631.5±8.71µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.1±0.44µs    16.5 MB/sec    1.03    183.7±4.14µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.02ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.2±1.01ms  1966.1 KB/sec    1.00     20.9±0.80ms  1992.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.25ms     3.0 MB/sec    1.00      5.5±0.35ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   719.5±57.55µs     4.1 MB/sec    1.00   700.8±62.16µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.1±0.39ms     2.8 MB/sec    1.00      9.0±0.38ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.69ms     4.0 MB/sec    1.15     11.7±0.80ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.5±0.20ms     6.7 MB/sec    1.00      2.4±0.10ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   279.7±20.23µs    10.5 MB/sec    1.02   284.0±16.68µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.25ms     5.0 MB/sec    1.06      5.3±0.40ms     4.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4948 on 2023-03-22 22:39_

Nit. This could be moved to a 'full_name' method on BindingKind to avoid repetition 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/scope.rs`:390 on 2023-03-22 22:41_

Thanks for commenting the fields. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/scope.rs`:433 on 2023-03-22 22:47_

Sounds good. I hoped to find a name in the python specification but didn't find any other than dotted name. What came to my mind is that this is the path but there's no precedence for this term

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/scope.rs`:453 on 2023-03-22 22:49_

Boxing a variant reduces the size of that variant to a single pointer instead of the size of the inner struct + padding.

I recommend boxing when one variant is significantly larger than all other variants. 

---

_@MichaReiser approved on 2023-03-22 22:50_

🎸🎸

---

_@charliermarsh reviewed on 2023-03-22 23:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4948 on 2023-03-22 23:05_

It doesn't exist on all variants though -- so would it return `Option<&str>`?

---

_Merged by @charliermarsh on 2023-03-22 23:08_

---

_Closed by @charliermarsh on 2023-03-22 23:08_

---

_Branch deleted on 2023-03-22 23:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4948 on 2023-03-23 06:06_

Yeah, it would have to return an Option

---

_@MichaReiser reviewed on 2023-03-23 06:06_

---
