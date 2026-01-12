```yaml
number: 3822
title: "Track star imports on `Scope` directly"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/stars
created_at: 2023-03-30T18:19:52Z
updated_at: 2023-03-31T15:20:00Z
url: https://github.com/astral-sh/ruff/pull/3822
synced_at: 2026-01-12T04:28:19Z
```

# Track star imports on `Scope` directly

---

_Pull request opened by @charliermarsh on 2023-03-30 18:19_

## Summary

Prior to this PR, when we saw a star import in the AST, like:

```py
from sys import *
```

We added a binding to the symbol table under the name `*`, with a kind of `StarImportation`. This had a number of strange consequences -- for example, if you have multiple star imports in the same scope, we only keep track of the "last" one.

This PR modifies the behavior to instead track the list of star imports on the `Scope`, rather than adding any bindings at all. To me, this better maps to the intent: we often want to know if we had any star imports to account for undefined variables, so we can now traverse over the list of star imports directly.

Note that, ideally, upon seeing `from sys import *`, we'd add an import binding for every _member_ in `sys`. But we don't have any way to detect the imported members, so for now, we don't add _any_ bindings.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-30 18:20_

---

_@charliermarsh reviewed on 2023-03-30 18:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4941 on 2023-03-30 18:20_

(This should only be done in the global scope.)

---

_Comment by @github-actions[bot] on 2023-03-30 18:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     15.7±0.87ms     2.6 MB/sec     1.05     16.4±1.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.16ms     4.3 MB/sec     1.04      4.0±0.14ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   505.0±34.76µs     5.8 MB/sec     1.03   521.8±18.44µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.37ms     3.9 MB/sec     1.04      6.9±0.34ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.28ms     5.2 MB/sec     1.05      8.2±0.24ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1916.8±115.31µs     8.7 MB/sec    1.00  1821.0±101.97µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.02   223.6±14.29µs    13.2 MB/sec     1.00   219.7±15.64µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.15ms     6.8 MB/sec     1.02      3.9±0.19ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     14.7±0.37ms     2.8 MB/sec    1.00     14.4±0.31ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.08ms     4.4 MB/sec    1.00      3.8±0.09ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.6±8.20µs     6.5 MB/sec    1.03   469.0±11.67µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.20ms     4.1 MB/sec    1.00      6.2±0.13ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.14ms     5.4 MB/sec    1.00      7.5±0.12ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1648.5±34.60µs    10.1 MB/sec    1.01  1662.3±29.63µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.8±5.94µs    16.5 MB/sec    1.01    180.4±5.72µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.5 MB/sec    1.01      3.4±0.07ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1197 on 2023-03-31 06:12_

Nit: It would be a good practice to start, in my view not exposing the internal data structures directly and instead, add public methods on `Scope` to add imports and read them.

Doing so eases replacing the internal representation later or uphold constraints that must be guaranteed (e.g. inserting in sorted order)

---

_@MichaReiser approved on 2023-03-31 06:16_

---

_@charliermarsh reviewed on 2023-03-31 14:55_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1197 on 2023-03-31 14:55_

Fixed :)

---

_Merged by @charliermarsh on 2023-03-31 15:01_

---

_Closed by @charliermarsh on 2023-03-31 15:01_

---

_Branch deleted on 2023-03-31 15:01_

---
