```yaml
number: 4791
title: " [`flake8-pyi`] Implement `PYI025`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PYI025
created_at: 2023-06-01T17:25:47Z
updated_at: 2023-06-02T14:43:22Z
url: https://github.com/astral-sh/ruff/pull/4791
synced_at: 2026-01-12T15:55:16Z
```

#  [`flake8-pyi`] Implement `PYI025`

---

_@qdegraaf_

## Summary

Add [Y025](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/pyi.py#L2063-L2066) from [flake8-pyi](https://github.com/PyCQA/flake8-pyi) to the plugin.  

## Test Plan

Added test fixtures for both stub and non-stub files

## Issue links

Refers: https://github.com/charliermarsh/ruff/issues/848


---

_Marked ready for review by @qdegraaf on 2023-06-01 17:25_

---

_Review comment by @qdegraaf on `crates/ruff/src/checkers/ast/mod.rs`:1025 on 2023-06-01 17:30_

@MichaReiser I will start doing this for any rules I implement now, as you have recommended it a couple times now, and I like the cleanliness of not having to break down all the required arguments for any given rule func. 

Just for my understanding/education. Is this a new feature that Rust/Ruff supports that was not there before, and that's why almost all other funcs expect a selection of structfields as arguments? Or is this just a preference thing that other devs have decided otherwise on?

If the former is true, would it be worthwhile to refactor all existing rules to accept a whole Stmt and pick the required fields in the rule func? If so I could create an Issue for it and maybe look at doing that depending on how much work that would be.

---

_@qdegraaf reviewed on 2023-06-01 17:30_

---

_Comment by @github-actions[bot] on 2023-06-01 18:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.1±0.81ms  1971.1 KB/sec    1.01     21.3±0.87ms  1957.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.25ms     3.5 MB/sec    1.00      4.8±0.23ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   597.0±34.80µs     4.9 MB/sec    1.02   608.0±36.55µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.34ms     3.0 MB/sec    1.00      8.4±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.37ms     4.1 MB/sec    1.03     10.1±1.14ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.1±0.16ms     7.9 MB/sec    1.00      2.1±0.11ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.3±10.69µs    12.1 MB/sec    1.04   253.7±17.64µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.4±0.20ms     5.8 MB/sec    1.00      4.3±0.24ms     5.9 MB/sec
parser/large/dataset.py                    1.00      7.5±0.26ms     5.4 MB/sec    1.04      7.8±0.25ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1477.3±64.23µs    11.3 MB/sec    1.04  1539.1±72.36µs    10.8 MB/sec
parser/numpy/globals.py                    1.00    147.1±7.22µs    20.1 MB/sec    1.08   158.3±12.01µs    18.6 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.11ms     7.8 MB/sec    1.04      3.4±0.15ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.21ms     2.3 MB/sec    1.01     17.5±0.30ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.07ms     3.8 MB/sec    1.00      4.4±0.07ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   513.4±17.98µs     5.7 MB/sec    1.00    510.9±8.38µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.14ms     3.5 MB/sec    1.01      7.3±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.8 MB/sec    1.01      8.5±0.10ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1803.6±24.80µs     9.2 MB/sec    1.01  1817.6±20.73µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.6±4.57µs    14.4 MB/sec    1.01    208.5±4.92µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.8±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.4±0.07ms     6.3 MB/sec    1.02      6.6±0.07ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1214.6±13.85µs    13.7 MB/sec    1.02  1240.8±15.71µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    124.8±2.69µs    23.6 MB/sec    1.02    127.6±2.15µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.08ms     9.2 MB/sec    1.01      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-06-01 19:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1025 on 2023-06-01 19:52_

It's a change we made to the AST structure. It used to be the case that `ast::StmtImportFrom` didn't exist at all, and instead, there was a single `StmtKind` enum, like:

```rust
enum StmtKind {
    ImportFrom {
        module: Option<Identifier>,
        names: Vec<Alias<R>>,
        level: Option<Int>,
    },
    Import {
        ...
    },
    ...
}
```

So, there was no way to pass "a statement that is an `ImportFrom` to a method" -- you could either pass a `Stmt`, which could then be _any_ kind of statement, or the individual fields (which is what we ended up doing in most places).

Going forward, it's better for rules to take (e.g.) `ast::StmtImportFrom`. I don't yet want to go about translating all the existing rules, because we're considering changing the APIs in some other ways (like #4564), and I'd rather do it all-at-once -- but I do consider it a better practice than passing the destructured fields.

---

_@qdegraaf reviewed on 2023-06-01 20:58_

---

_Review comment by @qdegraaf on `crates/ruff/src/checkers/ast/mod.rs`:1025 on 2023-06-01 20:58_

Thanks for the clear explanation! Will continue using the `ast::` kinds for future plugins and wait with anything else until there's news on the API change.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1025 on 2023-06-01 21:30_

We'll desperately need help with a migration once we've refined the API 😂 

---

_@charliermarsh reviewed on 2023-06-01 21:30_

---

_Renamed from " [flake8-pyi] Implement PYI025" to " [`flake8-pyi`] Implement `PYI025`" by @charliermarsh on 2023-06-01 22:36_

---

_Label `rule` added by @charliermarsh on 2023-06-01 22:36_

---

_Comment by @charliermarsh on 2023-06-01 22:37_

I had to remove the autofix, since if we rename the import, we also need to rename all usages.

---

_Comment by @charliermarsh on 2023-06-01 22:39_

(This is actually a good candidate for local symbol renaming.)

---

_Merged by @charliermarsh on 2023-06-01 22:45_

---

_Closed by @charliermarsh on 2023-06-01 22:45_

---

_Branch deleted on 2023-06-02 14:43_

---
