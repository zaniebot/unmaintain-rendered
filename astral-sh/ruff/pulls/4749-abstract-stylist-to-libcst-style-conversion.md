```yaml
number: 4749
title: Abstract stylist to libcst style conversion
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: codegen_state
created_at: 2023-05-31T09:02:47Z
updated_at: 2023-06-05T07:45:01Z
url: https://github.com/astral-sh/ruff/pull/4749
synced_at: 2026-01-12T15:55:16Z
```

# Abstract stylist to libcst style conversion

---

_@konstin_

Replace all duplicate invocations of

```rust
let mut state = CodegenState {
    default_newline: &stylist.line_ending(),
    default_indent: stylist.indentation(),
    ..CodegenState::default()
}
tree.codegen(&mut state);
state.to_string()
```

with

```rust
tree.codegen_stylist(&stylist)
```

No functional changes.

---

_@MichaReiser approved on 2023-05-31 09:21_

Looks good to me. I don't know if it was intentional that the `ast` crate doesn't depend on the `cst` crate.

---

_Comment by @github-actions[bot] on 2023-05-31 09:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.17ms     2.8 MB/sec    1.01     14.7±0.12ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    360.2±0.85µs     8.2 MB/sec    1.01    363.2±3.40µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.10ms     4.2 MB/sec    1.02      6.2±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.04ms     5.6 MB/sec    1.01      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1530.0±1.85µs    10.9 MB/sec    1.00   1534.9±3.45µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±0.59µs    17.9 MB/sec    1.00    164.9±1.70µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.01      3.3±0.00ms     7.7 MB/sec
parser/large/dataset.py                    1.06      6.2±0.01ms     6.6 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.05  1196.8±26.64µs    13.9 MB/sec    1.00   1134.7±0.72µs    14.7 MB/sec
parser/numpy/globals.py                    1.04    120.9±0.26µs    24.4 MB/sec    1.00    116.3±0.53µs    25.4 MB/sec
parser/pydantic/types.py                   1.05      2.6±0.00ms     9.7 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.33ms     2.4 MB/sec    1.00     17.0±0.28ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   495.9±14.60µs     6.0 MB/sec    1.00    489.7±6.74µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.10ms     3.6 MB/sec    1.00      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.12ms     4.8 MB/sec    1.00      8.3±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1760.5±27.14µs     9.5 MB/sec    1.00  1767.2±20.64µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.4±3.84µs    14.9 MB/sec    1.00    198.5±5.82µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.20ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.2 MB/sec    1.16      7.6±0.11ms     5.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1235.6±13.08µs    13.5 MB/sec    1.13  1399.6±23.15µs    11.9 MB/sec
parser/numpy/globals.py                    1.00    126.5±2.13µs    23.3 MB/sec    1.11    139.9±6.79µs    21.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.13      3.2±0.04ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-05-31 09:57_

> I don't know if it was intentional that the ast crate doesn't depend on the cst crate.

i already though about making this a free standing function in the ruff crate otherwise, @charliermarsh any preferences?

---

_Review requested from @charliermarsh by @konstin on 2023-05-31 09:57_

---

_Comment by @charliermarsh on 2023-05-31 15:23_

It wasn't intentional but it is a heavy dependency. Should we make it a feature?

---

_Comment by @konstin on 2023-06-01 07:00_

Current dependencies on/for this PR:
* main
  * **PR #4749** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4749" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #4777** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4777" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/4749?utm_source=stack-comment).

---

_Comment by @konstin on 2023-06-01 07:01_

To make this less hypothetical, this would be the alternative without the libcst dep: https://github.com/charliermarsh/ruff/pull/4777. If you want that version, you can merge it into this PR, if you don't want it you can close #4777 and merge this PR instead.

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/codemods.rs`:17 on 2023-06-01 21:00_

Does this just need a rebase? Why was this deleted?

---

_@charliermarsh approved on 2023-06-01 21:00_

---

_@konstin reviewed on 2023-06-01 21:15_

---

_Review comment by @konstin on `crates/ruff/src/autofix/codemods.rs`:17 on 2023-06-01 21:15_

i think the rebase went wrong, i have to fix that

---

_Comment by @konstin on 2023-06-02 14:48_

Found a better solution that abstracts away more code

---

_Review requested from @MichaReiser by @konstin on 2023-06-02 14:48_

---

_Comment by @charliermarsh on 2023-06-02 14:54_

Looks reasonable to me.

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/codemods.rs`:16 on 2023-06-05 07:02_

This seems to be sufficient
```suggestion
    fn codegen_stylist(&self, stylist: &'a Stylist) -> String;
```

---

_@MichaReiser approved on 2023-06-05 07:03_

---

_Merged by @konstin on 2023-06-05 07:22_

---

_Closed by @konstin on 2023-06-05 07:22_

---

_Branch deleted on 2023-06-05 07:22_

---
