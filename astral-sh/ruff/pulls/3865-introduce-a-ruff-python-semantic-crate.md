```yaml
number: 3865
title: "Introduce a `ruff_python_semantic` crate"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/semantic-crate
created_at: 2023-04-03T16:49:01Z
updated_at: 2023-04-04T17:07:11Z
url: https://github.com/astral-sh/ruff/pull/3865
synced_at: 2026-01-12T15:55:13Z
```

# Introduce a `ruff_python_semantic` crate

---

_@charliermarsh_

## Summary

This PR moves the following modules into a new `ruff_python_semantic` crate, to separate non-semantic AST responsibilities (`call_path.rs`, `newlines.rs`, etc.) from the semantic model, which will be captured in this crate:

- `binding.rs`
- `scope.rs`
- `context.rs`

There are a few additional modules that were in `ruff_python_ast` and depend on `Context`, but aren't part of the "semantic model" per se. For example, `visibility.rs` determines the visibility (public, private) of a given binding, and also has utilities for checking whether the declaration is abstract, static, etc. None of this is captured by the AST, so it requires semantic analysis. For now, I included these under `ruff_python_semantic::analyze`.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-04-03 16:49_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/logging.rs`:25 on 2023-04-03 16:49_

These are just definitions of types in the standard library, so they belong in `ruff_python_stdlib`.

---

_@charliermarsh reviewed on 2023-04-03 16:49_

---

_Comment by @github-actions[bot] on 2023-04-03 17:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.4±0.63ms     2.6 MB/sec    1.00     15.3±0.48ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.09ms     4.1 MB/sec    1.01      4.1±0.15ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   494.9±17.96µs     6.0 MB/sec    1.05   519.0±12.26µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.25ms     3.9 MB/sec    1.07      7.0±0.16ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.27ms     5.2 MB/sec    1.04      8.2±0.21ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1766.8±68.22µs     9.4 MB/sec    1.04  1841.6±45.30µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.5±5.89µs    14.9 MB/sec    1.03    204.8±5.35µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.12ms     7.1 MB/sec    1.05      3.8±0.09ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.54ms     2.1 MB/sec    1.00     19.7±0.55ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.20ms     3.3 MB/sec    1.00      5.0±0.16ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.03   617.6±36.49µs     4.8 MB/sec    1.00   599.5±29.03µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.27ms     3.1 MB/sec    1.00      8.2±0.25ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.03     10.2±0.26ms     4.0 MB/sec    1.00      9.9±0.33ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.07ms     7.5 MB/sec    1.00      2.2±0.07ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   236.3±14.36µs    12.5 MB/sec    1.01   239.1±12.21µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.15ms     5.7 MB/sec    1.01      4.5±0.17ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-04-04 06:47_

---

_Merged by @charliermarsh on 2023-04-04 16:50_

---

_Closed by @charliermarsh on 2023-04-04 16:50_

---

_Branch deleted on 2023-04-04 16:50_

---
