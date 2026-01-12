```yaml
number: 3788
title: "Move some generic structs out of `isort`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-structs
created_at: 2023-03-28T22:22:10Z
updated_at: 2023-03-30T12:58:04Z
url: https://github.com/astral-sh/ruff/pull/3788
synced_at: 2026-01-12T04:39:45Z
```

# Move some generic structs out of `isort`

---

_Pull request opened by @charliermarsh on 2023-03-28 22:22_

## Summary

These will be reused for "autofixes that require imports", so moving them here to keep the downstream PR focused.

---

_Comment by @github-actions[bot] on 2023-03-28 22:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.8±0.09ms     2.3 MB/sec    1.00     17.7±0.13ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.03ms     3.7 MB/sec    1.00      4.5±0.04ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    584.1±4.60µs     5.1 MB/sec    1.00    581.4±1.65µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.04ms     3.4 MB/sec    1.00      7.5±0.05ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      9.1±0.06ms     4.4 MB/sec    1.00      9.1±0.05ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.00ms     8.1 MB/sec    1.00      2.0±0.01ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    225.4±2.21µs    13.1 MB/sec    1.00    224.9±0.47µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.7±0.75ms     2.1 MB/sec    1.00     19.6±0.80ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.4±0.26ms     3.1 MB/sec    1.00      5.3±0.17ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   631.6±38.69µs     4.7 MB/sec    1.00   627.7±25.13µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.40ms     3.0 MB/sec    1.01      8.6±0.47ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.04     10.6±0.35ms     3.8 MB/sec    1.00     10.2±0.41ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.08ms     7.2 MB/sec    1.00      2.3±0.09ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.02   268.7±13.25µs    11.0 MB/sec    1.00   263.3±13.48µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.06      5.0±0.36ms     5.1 MB/sec    1.00      4.7±0.24ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-03-29 07:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/symbol_imports.rs`:3 on 2023-03-29 07:05_

We have a different understanding of the term `symbol`. In my view, these are possible `declarations` of a symbol. I'll add a more in-depth explanation in #3777 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/symbol_imports.rs`:18 on 2023-03-29 07:06_

Nit: `usize` is copy. Storying a reference to a `usize` takes as much storage as storing the `usize` directly. It should also be safe to use a `u32`. I don't think any valid python program (that is less than 4GB in size) can have more than `u32` levels :scream: 
```suggestion
    pub level: Option<u32>,
```

---

_@MichaReiser reviewed on 2023-03-29 07:06_

---

_@MichaReiser reviewed on 2023-03-29 07:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/symbol_imports.rs`:10 on 2023-03-29 07:10_

Nit: It's best practice to implement `Debug` and `Clone` for all public types (and other common traits if appropriate)

https://rust-lang.github.io/api-guidelines/interoperability.html#types-eagerly-implement-common-traits-c-common-traits

---

_@charliermarsh reviewed on 2023-03-29 22:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/symbol_imports.rs`:18 on 2023-03-29 22:14_

It's a little painful to change to `u32` as long as RustPython's `ImportFrom` uses `usize` here :/

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-29 22:19_

---

_@MichaReiser approved on 2023-03-30 05:25_

---

_Merged by @charliermarsh on 2023-03-30 12:58_

---

_Closed by @charliermarsh on 2023-03-30 12:58_

---

_Branch deleted on 2023-03-30 12:58_

---
