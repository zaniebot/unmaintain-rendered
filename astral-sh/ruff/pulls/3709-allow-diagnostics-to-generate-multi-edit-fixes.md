```yaml
number: 3709
title: Allow diagnostics to generate multi-edit fixes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/multiple-edit
created_at: 2023-03-24T03:43:40Z
updated_at: 2023-04-13T16:39:15Z
url: https://github.com/astral-sh/ruff/pull/3709
synced_at: 2026-01-12T15:55:13Z
```

# Allow diagnostics to generate multi-edit fixes

---

_@charliermarsh_

## Summary

This PR extends the autofix API to support multiple edits by introducing a basic `Fix` struct:

```rust
pub struct Fix {
    edits: Vec<Edit>,
}
```

...and propagating those changes throughout the codebase.

To demonstrate its usefulness, the `crates/ruff/src/rules/pyupgrade/rules/replace_stdout_stderr.rs` rule was also refactored to leverage this new API. (That rule needs to remove two keyword arguments, which may or may not be adjacent, and replace them with `capture_output=True`. Previously, this rule required that we included all intermediary content in the text edit. Now, we can model it as a replacement and a deletion.)

There are a few changes that need to be made downstream, to tools that depend on our JSON API (namely, the playground and the LSP, the latter of which can be done in a backwards-compatible way).


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-24 04:12_

---

_Review requested from @konstin by @charliermarsh on 2023-03-24 04:12_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:17 on 2023-03-24 04:15_

There's some awkwardness to this struct... I wish these constraints were expressed in the type system (or, at least, a subset of them). It'd also be nice if we didn't require a `Vec` here at all. The vast majority of fixes will have exactly one edit. Others, in practice, will have a bounded number of edits (maybe two? three?).

We could use `smallvec` to avoid heap allocations. I considered instead using an enum, like:

```rust
pub enum Fix {
  One(Edit),
  Two(Edit, Edit),
  ...
```

...and so on.

Would love feedback, this feels like a good learning opportunity for me.


---

_@charliermarsh reviewed on 2023-03-24 04:15_

---

_Marked ready for review by @charliermarsh on 2023-03-24 04:16_

---

_Comment by @charliermarsh on 2023-03-24 04:16_

The test fixture updates have _intentionally_ omitted for now, to keep this PR reviewable. (All test fixtures will change in a mechanical way, as we're moving from a single fix to a vector of fixes.)

---

_Comment by @github-actions[bot] on 2023-03-24 04:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.6±0.30ms     2.6 MB/sec    1.01     15.7±0.30ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.10ms     4.1 MB/sec    1.02      4.1±0.11ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   564.9±14.80µs     5.2 MB/sec    1.00   564.0±17.22µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.14ms     3.7 MB/sec    1.02      7.0±0.15ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.13ms     4.9 MB/sec    1.00      8.2±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1882.3±46.47µs     8.8 MB/sec    1.00  1869.1±39.64µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    209.5±6.30µs    14.1 MB/sec    1.00    209.1±7.25µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.08ms     6.5 MB/sec    1.00      3.9±0.10ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     15.8±0.21ms     2.6 MB/sec    1.00     15.2±0.18ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.07ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    534.8±8.19µs     5.5 MB/sec    1.00    528.1±6.54µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.9±0.09ms     3.7 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.06      8.7±0.07ms     4.7 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1889.4±29.80µs     8.8 MB/sec    1.00  1808.3±26.49µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    208.4±6.66µs    14.2 MB/sec    1.00    203.5±7.27µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.0±0.05ms     6.4 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:18 on 2023-03-24 07:54_

Nit: I learned that you can write
```suggestion
ruff_diagnostics.path = "../ruff_diagnostics"
ruff_python_ast.path = "../ruff_python_ast" 
ruff_rustpython.path = "../ruff_rustpython"
```

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:17 on 2023-03-24 08:04_

What's the motivation for enforcing that the edits are not empty? Does it simplify the consumption of edits downstream? 

I'm asking because `Fix::none` and the derived `Default` implementation generate empty fixes and the API provides an `is_none` function, making me believe this invariant isn't necessary (other than accidental creation of `Fix`es without any edits).

We would need to profile to know whether it makes sense to use smallvec or an enum with `Empty`, `One` and `Many` variants. My intuition is that it shouldn't matter much for projects that have adopted Ruff because they normally have most issues fixed (a few issues from the now-edited files and ignored suppressions). It may be different for projects that run ruff the first time but using an enum instead of a `Vec` has additional costs when reading the values (an iterator needs to determine the right variant in every `next` call) and may be less optimized. This means we would be trading one cost for another. 

I think it could be more interesting to use a single edits `Vec` for all changes in a file and uses indices into that vec to get the edits of a single fix. But again, I don't this is necessary just now.

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:33 on 2023-03-24 08:05_

This will panic on `Fix::default` and `Fix::none()`. It also isn't clear for a caller what to expect as the result of `fix.location()`. That's why I would change the method to

```suggestion
    pub fn first_location(&self) -> Option<Location> {
        self.edits.first().copied()
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:28 on 2023-03-24 08:06_

Nit: I would prefer `Fix::is_empty()` and `Fix::empty()`
```suggestion
    pub const fn empty() -> Self {
        Self::default()
    }

    pub const fn is_empty(&self) -> bool {
        self.edits.is_empty()
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:58 on 2023-03-24 08:07_

Nit: If this is used frequently, then I recommend adding a 

```
pub fn from_edit(edit: Edit) -> Self {
}
```

function to `Fix` to make it more explicit. I always find `From` implementations hard to discover. 

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:45 on 2023-03-24 08:08_

Nit: We could implement `FromIter` so that it is possible to create a `Fix` from any `Iter<Item=Edit>` so that you can write

```rust
let fix: Fix = function_that_generates_edits().collect();
```

---

_@MichaReiser approved on 2023-03-24 08:11_

---

_Review comment by @konstin on `crates/ruff_wasm/Cargo.toml`:18 on 2023-03-24 08:20_

to me the string-or-braces-with-details style feels more intuitive

---

_@konstin reviewed on 2023-03-24 08:20_

---

_@konstin reviewed on 2023-03-24 08:43_

---

_Review comment by @konstin on `crates/ruff_diagnostics/src/fix.rs`:33 on 2023-03-24 08:43_

looking at crates/ruff/src/autofix/mod.rs, shouldn't location be a new location (min of all starts, max of all ends), or maybe even something defined by the rule that selects the whole expression that is fixed so that we don't combine potentially incorrect fixes on the same expression?

---

_@charliermarsh reviewed on 2023-03-25 16:23_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/fix.rs`:17 on 2023-03-25 16:23_

All makes sense.

I think I'm getting hung up on the `Option<Vec<...>>` thing, whereby using an empty `Vec` is seen as preferable to using `None`. Because of that convention, at the level above, I was going to use ` Fix { edits: vec![] }` to represent "no fix", rather than `None`. But that creates some awkwardness in the API.


---

_Merged by @charliermarsh on 2023-03-26 20:45_

---

_Closed by @charliermarsh on 2023-03-26 20:45_

---

_Branch deleted on 2023-03-26 20:45_

---

_Comment by @fannheyward on 2023-04-13 05:21_

@charliermarsh sorry to boring you on this, but this PR changes the `ruff --format=json` format, I think we should mention this in BREAKING_CHANGES.md.

The reason I mention this is, coc-pyright uses ruff's fix info to fix codes, but the format has been changed in this PR, coc-pyright will change to the new output format.

Thank you for your work on ruff!

---

_Comment by @charliermarsh on 2023-04-13 16:39_

@fannheyward - You're 100% right, I'm not sure how I missed that -- purely an oversight. I'll add it to `BREAKING_CHANGES.md`. Sorry about that, and thank you for supporting Ruff!

---
