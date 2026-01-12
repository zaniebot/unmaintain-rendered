```yaml
number: 3766
title: "Fix `cargo test --doc`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix_cargo_test_doc
created_at: 2023-03-28T11:30:51Z
updated_at: 2023-03-28T12:29:26Z
url: https://github.com/astral-sh/ruff/pull/3766
synced_at: 2026-01-12T15:55:13Z
```

# Fix `cargo test --doc`

---

_@konstin_

See https://github.com/charliermarsh/ruff/issues/3752#issuecomment-1484243217

I don't understand why this didn't show up earlier, i thought `cargo test` would include `cargo test --doc`

---

_Review requested from @MichaReiser by @konstin on 2023-03-28 11:30_

---

_Review requested from @charliermarsh by @konstin on 2023-03-28 11:30_

---

_Marked ready for review by @konstin on 2023-03-28 11:30_

---

_@charliermarsh approved on 2023-03-28 11:32_

---

_Merged by @konstin on 2023-03-28 11:36_

---

_Closed by @konstin on 2023-03-28 11:36_

---

_Comment by @MichaReiser on 2023-03-28 11:43_

That's because `ruff_macros` disabled `doctests`

https://github.com/charliermarsh/ruff/blob/f68c26a5063136d7839988ea72390953127ae9fe/crates/ruff_macros/Cargo.toml#L8-L10

Not sure why

---

_Comment by @github-actions[bot] on 2023-03-28 11:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.12ms     2.5 MB/sec    1.00     16.3±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    552.7±4.35µs     5.3 MB/sec    1.00    553.6±3.62µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.05ms     4.8 MB/sec    1.00      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1927.0±13.30µs     8.6 MB/sec    1.00  1923.5±10.21µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.0±2.12µs    13.8 MB/sec    1.00    214.3±2.75µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.03ms     6.4 MB/sec    1.00      3.9±0.03ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     20.2±0.89ms     2.0 MB/sec    1.00     19.1±1.01ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.0±0.17ms     3.3 MB/sec    1.00      4.7±0.19ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.05   625.4±21.02µs     4.7 MB/sec    1.00   595.7±28.63µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.05      8.3±0.46ms     3.1 MB/sec    1.00      7.9±0.32ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.07     10.0±0.44ms     4.1 MB/sec    1.00      9.3±0.31ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05      2.2±0.10ms     7.4 MB/sec    1.00      2.1±0.11ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    242.7±9.71µs    12.2 MB/sec    1.03   250.9±11.91µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.4±0.18ms     5.8 MB/sec    1.00      4.4±0.24ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-03-28 11:54_

---

_Comment by @konstin on 2023-03-28 11:54_

oh interesting! The source is #1772. Though now i'm even more confused as to why `cargo test --doc` ignores the flag

---

_Comment by @MichaReiser on 2023-03-28 12:29_

> oh interesting! The source is #1772. Though now i'm even more confused as to why `cargo test --doc` ignores the flag

I understand the `doctest=false` configuration tells `cargo` this crate has no doctests. Meaning, it's not necessary to parse the files, extract and compile the doctests for this crate. 

---
