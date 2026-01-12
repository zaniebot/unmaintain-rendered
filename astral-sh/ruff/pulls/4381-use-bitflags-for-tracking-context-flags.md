```yaml
number: 4381
title: "Use `bitflags` for tracking `Context` flags"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bitflags
created_at: 2023-05-11T20:39:10Z
updated_at: 2023-05-12T20:23:08Z
url: https://github.com/astral-sh/ruff/pull/4381
synced_at: 2026-01-12T15:55:15Z
```

# Use `bitflags` for tracking `Context` flags

---

_@charliermarsh_

## Summary

`Context` has accumulated a bunch of boolean flags over its lifetime, to track state as we traverse over the AST.

This PR consolidates those flags into a single `ContextFlags` bit vector, and streamlines some of the logic around setting and restoring flags throughout the visitor. In particular, we now save the flags prior to recursing into any node, and restore them when we finish the subtree traversal, which means that we don't have to track individual flags and turn them off-and-on as we iterate over the tree.

Changing to a cheap bit vector also means that it's very inexpensive for us to save and restore the _entire_ `ContextFlags` state when we process our deferred AST nodes. Previously, we selectively saved-and-restored specific boolean flags (like `in_type_checking_block`). Now, we just save the entire `ContextFlags`, which is much more "correct", since we're no longer discarding state.

## Test Plan

Good candidate for automated testing given that this is intended to be a no-op refactor.

From a performance standpoint, my testing indicates that there's no significant change from `main`, which is good:

```
linter/default-rules/numpy/globals.py
                        time:   [72.565 µs 72.586 µs 72.608 µs]
                        thrpt:  [40.638 MiB/s 40.651 MiB/s 40.662 MiB/s]
                 change:
                        time:   [-0.4019% -0.2403% -0.0927%] (p = 0.00 < 0.05)
                        thrpt:  [+0.0928% +0.2409% +0.4036%]
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  3 (3.00%) low severe
  3 (3.00%) high mild
  2 (2.00%) high severe
linter/default-rules/pydantic/types.py
                        time:   [1.4602 ms 1.4607 ms 1.4613 ms]
                        thrpt:  [17.452 MiB/s 17.460 MiB/s 17.466 MiB/s]
                 change:
                        time:   [-0.1825% -0.0945% -0.0049%] (p = 0.04 < 0.05)
                        thrpt:  [+0.0049% +0.0946% +0.1828%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  1 (1.00%) low severe
  2 (2.00%) high mild
  3 (3.00%) high severe
linter/default-rules/numpy/ctypeslib.py
                        time:   [666.52 µs 666.75 µs 667.00 µs]
                        thrpt:  [24.964 MiB/s 24.973 MiB/s 24.982 MiB/s]
                 change:
                        time:   [-0.4702% -0.3496% -0.2350%] (p = 0.00 < 0.05)
                        thrpt:  [+0.2356% +0.3508% +0.4724%]
                        Change within noise threshold.
Found 10 outliers among 100 measurements (10.00%)
  2 (2.00%) low severe
  6 (6.00%) high mild
  2 (2.00%) high severe
linter/default-rules/large/dataset.py
                        time:   [3.3361 ms 3.3368 ms 3.3377 ms]
                        thrpt:  [12.189 MiB/s 12.192 MiB/s 12.195 MiB/s]
                 change:
                        time:   [+0.0435% +0.1154% +0.1823%] (p = 0.00 < 0.05)
                        thrpt:  [-0.1820% -0.1153% -0.0435%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) high mild
  4 (4.00%) high severe

linter/all-rules/numpy/globals.py
                        time:   [146.28 µs 146.37 µs 146.45 µs]
                        thrpt:  [20.148 MiB/s 20.159 MiB/s 20.171 MiB/s]
                 change:
                        time:   [+0.1531% +0.5829% +1.0416%] (p = 0.00 < 0.05)
                        thrpt:  [-1.0308% -0.5795% -0.1529%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) low severe
  1 (1.00%) low mild
  2 (2.00%) high mild
  1 (1.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.8998 ms 2.9012 ms 2.9028 ms]
                        thrpt:  [8.7858 MiB/s 8.7906 MiB/s 8.7947 MiB/s]
                 change:
                        time:   [+0.1964% +0.3161% +0.4412%] (p = 0.00 < 0.05)
                        thrpt:  [-0.4392% -0.3151% -0.1960%]
                        Change within noise threshold.
Found 12 outliers among 100 measurements (12.00%)
  2 (2.00%) low severe
  6 (6.00%) low mild
  4 (4.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.6589 ms 1.6617 ms 1.6644 ms]
                        thrpt:  [10.005 MiB/s 10.021 MiB/s 10.038 MiB/s]
                 change:
                        time:   [-0.0394% +0.1574% +0.3722%] (p = 0.15 > 0.05)
                        thrpt:  [-0.3708% -0.1571% +0.0394%]
                        No change in performance detected.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/all-rules/large/dataset.py
                        time:   [6.8793 ms 6.8810 ms 6.8832 ms]
                        thrpt:  [5.9105 MiB/s 5.9123 MiB/s 5.9138 MiB/s]
                 change:
                        time:   [-0.0697% +0.0027% +0.0718%] (p = 0.94 > 0.05)
                        thrpt:  [-0.0717% -0.0027% +0.0698%]
                        No change in performance detected.
Found 15 outliers among 100 measurements (15.00%)
  2 (2.00%) low severe
  3 (3.00%) low mild
  2 (2.00%) high mild
  8 (8.00%) high severe

     Running benches/parser.rs (target/release/deps/parser-856fa5246b0932f7)
```


---

_Comment by @github-actions[bot] on 2023-05-11 20:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.6±0.54ms     2.6 MB/sec    1.01     15.8±1.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.12ms     4.4 MB/sec    1.00      3.8±0.12ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   479.6±27.01µs     6.2 MB/sec    1.01   482.8±17.83µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.7±0.28ms     3.8 MB/sec    1.00      6.5±0.21ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.31ms     5.4 MB/sec    1.00      7.5±0.21ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1612.6±65.84µs    10.3 MB/sec    1.00  1581.7±44.42µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.9±8.26µs    15.5 MB/sec    1.05   198.9±15.61µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.12ms     7.5 MB/sec    1.01      3.4±0.13ms     7.5 MB/sec
parser/large/dataset.py                    1.00      6.0±0.18ms     6.7 MB/sec    1.01      6.1±0.19ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1177.8±37.17µs    14.1 MB/sec    1.02  1203.2±50.66µs    13.8 MB/sec
parser/numpy/globals.py                    1.01    120.0±6.61µs    24.6 MB/sec    1.00    118.9±6.61µs    24.8 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.11ms     9.7 MB/sec    1.01      2.6±0.14ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.8±0.19ms     2.4 MB/sec    1.00     16.5±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    491.3±6.95µs     6.0 MB/sec    1.00    485.4±5.82µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.1±0.08ms     3.6 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.3±0.12ms     4.9 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1774.4±24.81µs     9.4 MB/sec    1.00  1693.8±14.06µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    194.5±7.37µs    15.2 MB/sec    1.00    191.7±4.49µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.8±0.06ms     6.8 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.6±0.06ms     6.2 MB/sec    1.00      6.6±0.06ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1240.2±14.09µs    13.4 MB/sec    1.01  1254.5±11.79µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    126.9±1.75µs    23.3 MB/sec    1.01    127.9±2.41µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.02      2.9±0.05ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-11 21:43_

---

_Review requested from @konstin by @charliermarsh on 2023-05-11 21:43_

---

_Marked ready for review by @charliermarsh on 2023-05-11 21:43_

---

_@charliermarsh reviewed on 2023-05-11 21:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:30 on 2023-05-11 21:44_

(Decided to move this to the bottom of the file along with the `ContextFlags`.)

---

_@charliermarsh reviewed on 2023-05-11 21:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:144 on 2023-05-11 21:45_

Note: we're exposing the flags to enable the visitor to modify them, rather than exposing a bunch of write methods, which felt tedious to me. Open to input though.

---

_@charliermarsh reviewed on 2023-05-11 21:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1154 on 2023-05-11 21:46_

This got moved "up" into an earlier phase in `visit_stmt` -- the phase where we check if we're past the `__future__` and `import` boundaries.

The nice thing about this separation is that _only_ this first phase modifies any permanent flags. All subsequent modifications are local to the subtree, so we can save the current flags and restore them at the end of the visitor.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2276 on 2023-05-11 21:46_

So the new flow is: we save the flags, then recurse and set any necessary flags, then restore the previous flags when we finish the recursion phase (end of `visit_expr`).

---

_@charliermarsh reviewed on 2023-05-11 21:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:534 on 2023-05-12 06:09_

Would you mind documenting the 
meaning of each flag. Ideally with a small python code example 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:700 on 2023-05-12 06:09_

What's the size of this type? Should it be copy?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:144 on 2023-05-12 07:12_

What's the reason for this being a macro vs a regular method or standalone function that takes `ctx` as an argument?

```rust
impl<'_> Checker<'_> {
    fn visit_type_checking_block(&mut self, body: &[Stmt]) {
        let flags = self.ctx.flags;
        self.ctx.flags |= ContextFlags::TYPE_CHECKING_BLOCK;
        self.visit_body(body);
        self.ctx.flags = flags;
    }
}
```

The same applies for the other macros (`visit_type_definition`...)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:201 on 2023-05-12 07:14_

`FUTURES_BOUNDARY` seems new. What's the reason for introducing it?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:225 on 2023-05-12 07:15_

Can we add a comment here why it is necessary to save the flags here? I assume it is because the visiting mutates the flag? How about `flag_snapshot` to make it clear that the sole purpose is to copy the flags to restore them later.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2276 on 2023-05-12 07:19_

What would be nice is to make use of Rust's `Drop` to restore the flags at the end but I think that won't work because it would require mutably borrowing `ctx` multiple times. 

We worked around this in Rome by wrapping the `Parser` but I think that would, again, require significant refactors to make it work here

https://github.com/rome/tools/blob/38104b339da8ff688f469799e1fc3f4a46f3d2ec/crates/rome_js_parser/src/state.rs#L263-L300

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:3881 on 2023-05-12 07:20_

Why is it no longer required to remove the `ContextFlags::F_STRING` at the end? Is it because we restore all flags at the end?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4793 on 2023-05-12 07:21_

Nit: 

```suggestion
                self.ctx.flags |= ContextFlags::TYPE_DEFINITION | ContextFlags::FUTURE_TYPE_DEFINITION;
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4825 on 2023-05-12 07:22_

Nit: Could be worth storing the `string_type_definition_flag` in a variable and then reuse it on line 4793

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:399 on 2023-05-12 07:23_

`intersects` is not `const`... interersting

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:446 on 2023-05-12 07:24_

Nit: Is it possible to make the methods `const`?

---

_@MichaReiser approved on 2023-05-12 07:24_

---

_Review comment by @konstin on `crates/ruff/src/checkers/ast/mod.rs`:2292 on 2023-05-12 07:39_

TIL their implementation of `SubAssign` is actually `self.bits & !other.bits` (which makes sense, it's just asymmetric with the lack of `AddAssign`)

---

_Review comment by @konstin on `crates/ruff_python_semantic/src/context.rs`:47 on 2023-05-12 07:40_

nice

---

_@konstin approved on 2023-05-12 07:41_

---

_@charliermarsh reviewed on 2023-05-12 12:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:144 on 2023-05-12 12:47_

I honestly can't remember, I assume there's some kind of Chesterton's Fence thing here but I will revisit it in a follow-up PR.

---

_@charliermarsh reviewed on 2023-05-12 12:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:201 on 2023-05-12 12:48_

It's the same as `futures_allowed`, but renamed for consistency with `IMPORT_BOUNDARY` -- have we seen the future-imports boundary? Or are future imports still ok? (Will add docs for all of these as per above.)

---

_@charliermarsh reviewed on 2023-05-12 15:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:3881 on 2023-05-12 15:45_

Yes.

---

_@charliermarsh reviewed on 2023-05-12 15:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:700 on 2023-05-12 15:47_

It's already `Copy`, so I'll take a look at this in a subsequent PR.

---

_Merged by @charliermarsh on 2023-05-12 16:07_

---

_Closed by @charliermarsh on 2023-05-12 16:07_

---

_Branch deleted on 2023-05-12 16:07_

---

_@MichaReiser reviewed on 2023-05-12 20:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:144 on 2023-05-12 20:17_

A what 😅?

---

_@charliermarsh reviewed on 2023-05-12 20:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:144 on 2023-05-12 20:23_

Hahaha. You come across a fence, and you're not sure why it's there. But you probably don't want to tear it down without understanding why it exists :D

---
