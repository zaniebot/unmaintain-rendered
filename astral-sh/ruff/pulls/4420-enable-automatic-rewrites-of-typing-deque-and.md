```yaml
number: 4420
title: "Enable automatic rewrites of `typing.Deque` and `typing.DefaultDict`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/deque
created_at: 2023-05-13T19:42:37Z
updated_at: 2023-05-15T22:58:17Z
url: https://github.com/astral-sh/ruff/pull/4420
synced_at: 2026-01-12T03:50:02Z
```

# Enable automatic rewrites of `typing.Deque` and `typing.DefaultDict`

---

_Pull request opened by @charliermarsh on 2023-05-13 19:42_

## Summary

This PR leverages our import-dependent autofix machinery to enable replacing usages of `typing.Deque` with `collections.deque`, which is available as a standard library generic since Python 3.9 (or usable earlier with `from __future__ import annotations`).

There are a few other symbols we can rewrite as well, but `typing.Deque` and `typing.DefaultDict` are the most common, so I want to start with those.

Closes #3510.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-13 19:42_

---

_Review request for @MichaReiser removed by @charliermarsh on 2023-05-13 19:42_

---

_Review requested from @konstin by @charliermarsh on 2023-05-13 19:42_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-13 19:42_

---

_Label `autofix` added by @charliermarsh on 2023-05-13 19:42_

---

_Comment by @github-actions[bot] on 2023-05-13 19:48_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.04ms     2.9 MB/sec    1.02     14.2±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.3±1.28µs     7.1 MB/sec    1.00    419.5±1.09µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.0 MB/sec    1.02      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1449.5±3.55µs    11.5 MB/sec    1.02   1476.2±4.95µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.3±0.61µs    18.5 MB/sec    1.01    161.6±0.28µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.4±0.01ms     7.5 MB/sec    1.00      5.3±0.02ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1049.5±0.65µs    15.9 MB/sec    1.00   1038.4±1.84µs    16.0 MB/sec
parser/numpy/globals.py                    1.00    106.1±0.20µs    27.8 MB/sec    1.00    106.3±1.81µs    27.8 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.1±0.38ms     2.4 MB/sec    1.00     16.8±0.48ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.4±0.26ms     3.8 MB/sec    1.00      4.2±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   509.7±25.06µs     5.8 MB/sec    1.00   500.0±14.89µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.19ms     3.6 MB/sec    1.00      7.1±0.36ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.26ms     4.8 MB/sec    1.00      8.3±0.20ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1778.0±52.09µs     9.4 MB/sec    1.00  1753.5±41.79µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   200.5±13.00µs    14.7 MB/sec    1.00    198.3±7.34µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.10ms     6.8 MB/sec    1.00      3.7±0.08ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.15ms     6.0 MB/sec    1.02      6.9±0.24ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1281.8±43.30µs    13.0 MB/sec    1.04  1332.4±51.54µs    12.5 MB/sec
parser/numpy/globals.py                    1.00    131.4±5.15µs    22.5 MB/sec    1.01    132.4±6.54µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.11ms     8.9 MB/sec    1.01      2.9±0.09ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/use_pep585_annotation.rs`:69 on 2023-05-14 00:25_

You could choose an applicability :)

---

_@zanieb reviewed on 2023-05-14 00:25_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP006.py.snap`:22 on 2023-05-15 07:35_

The recommendation here doesn't match the code (the code only uses `List` not `typing.List`) which can be confusing to users because they don't recognize their own code.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP006.py.snap`:6 on 2023-05-15 07:36_

Should this fix remove the `typing` import if it was the last usages or do we leave this to the unused imports lint rule?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/use_pep585_annotation.rs`:67 on 2023-05-15 07:37_

What are the semantic changes that this fix can introduce? Does importing `collections` have any side effects? Or is it just because we feel less confident about fixes that patch up imports?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:87 on 2023-05-15 07:39_

Nit: Should `ModuleMember` be an `enum`? 

```rust
enum ModuleMember<'a> {
	BuiltIn { name: &'a str },
	Member { module: &'a str, member: &'a str }
}
```

---

_@MichaReiser approved on 2023-05-15 07:40_

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/rules/use_pep585_annotation.rs`:14 on 2023-05-15 11:10_

```suggestion
    /// Existing non-PEP 585 annotation
    from: ModuleMember<'static>,
    /// Rewritten PEP 585 annotation
    to: ModuleMember<'static>,
```

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP006.py.snap`:22 on 2023-05-15 11:51_

rustc switches depending on whether symbol is ambiguous:

Different names:

```rust
fn a(b: String) -> String {
    return b;
}

fn main() {
    struct String2;
    let x = a(String2);
    println!("Hello, {}!", x);
}
```

```
error[E0308]: mismatched types
 --> src/main.rs:7:15
  |
7 |     let x = a(String2);
  |             - ^^^^^^^ expected `String`, found `String2`
  |             |
  |             arguments to this function are incorrect
  |
note: function defined here
 --> src/main.rs:1:4
  |
1 | fn a(b: String) -> String {
  |    ^ ---------
```

```rust
fn a(b: String) -> String {
    return b;
}

fn main() {
    struct String;
    let x = a(String);
    println!("Hello, {}!", x);
}
```

Same names:

```
error[E0308]: mismatched types
   --> src/main.rs:7:15
    |
7   |     let x = a(String);
    |             - ^^^^^^ expected `std::string::String`, found `main::String`
    |             |
    |             arguments to this function are incorrect
    |
    = note: `main::String` and `std::string::String` have similar names, but are actually distinct types
note: `main::String` is defined in the current crate
   --> src/main.rs:6:5
    |
6   |     struct String;
    |     ^^^^^^^^^^^^^
note: `std::string::String` is defined in crate `alloc`
   --> /home/konsti/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/string.rs:365:1
    |
365 | pub struct String {
    | ^^^^^^^^^^^^^^^^^
note: function defined here
   --> src/main.rs:1:4
    |
1   | fn a(b: String) -> String {
    |    ^ ---------
```



---

_@konstin approved on 2023-05-15 11:57_

---

_Comment by @konstin on 2023-05-15 11:58_

test failure is due to outdated snapshots

---

_@konstin approved on 2023-05-15 13:18_

---

_@charliermarsh reviewed on 2023-05-15 13:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP006.py.snap`:6 on 2023-05-15 13:41_

We leave it to the unused imports lint.

---

_Comment by @charliermarsh on 2023-05-15 13:42_

I need to rebase as well -- the `flake8-future-annotations` changes will require some edits here.

---

_@charliermarsh reviewed on 2023-05-15 22:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep585_annotation.rs`:67 on 2023-05-15 22:26_

It's mostly the latter -- I just want to improve the import-patching fixes before elevating them.

---

_Merged by @charliermarsh on 2023-05-15 22:33_

---

_Closed by @charliermarsh on 2023-05-15 22:33_

---

_Branch deleted on 2023-05-15 22:33_

---
