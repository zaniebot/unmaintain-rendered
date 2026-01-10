```yaml
number: 18711
title: "[ty] Use more parallelism when running corpus tests"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/split-corpus-tests-more
created_at: 2025-06-16T14:39:24Z
updated_at: 2025-06-16T17:38:56Z
url: https://github.com/astral-sh/ruff/pull/18711
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Use more parallelism when running corpus tests

---

_Pull request opened by @AlexWaygood on 2025-06-16 14:39_

## Summary

Now that we've moved the corpus tests to the `ty_python_semantic` crate, running `cargo test -p ty_python_semantic` takes an annoying amount of time locally. (This also hasn't been helped by the fact that we run a lot more corpus tests after https://github.com/astral-sh/ruff/pull/18531.) We can speed things up quite a bit by splitting the corpus tests up more, so that they run in parallel.

## Test Plan

On `main` locally:

```
~/dev/ruff (main)⚡ % cargo test -p ty_python_semantic --test corpus              
   Compiling ty_python_semantic v0.0.0 (/Users/alexw/dev/ruff/crates/ty_python_semantic)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.81s
     Running tests/corpus.rs (target/debug/deps/corpus-1d987a322669ccb0)

running 6 tests
test corpus_no_panic ... ok
test linter_stubs_no_panic ... ok
test parser_no_panic ... ok
test linter_af_no_panic ... ok
test linter_gz_no_panic ... ok
test typeshed_no_panic ... ok

test result: ok. 6 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 8.63s
```

On this PR branch locally:

```
~/dev/ruff (alex/split-corpus-tests-more)⚡ % cargo test -p ty_python_semantic --test corpus
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running tests/corpus.rs (target/debug/deps/corpus-1d987a322669ccb0)

running 12 tests
test corpus_no_panic ... ok
test linter_no_panic::_a_e_expects ... ok
test linter_no_panic::_g_o_expects ... ok
test linter_stubs_no_panic ... ok
test parser_no_panic ... ok
test typeshed_no_panic::_f_k_expects ... ok
test typeshed_no_panic::_l_p_expects ... ok
test typeshed_no_panic::_a_e_expects ... ok
test linter_no_panic::_q_z_expects ... ok
test typeshed_no_panic::_q_z_expects ... ok
test linter_no_panic::_p_expects ... ok
test linter_no_panic::_f_expects ... ok

test result: ok. 12 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 3.77s
```

I.e., a speedup of around 5s

---

_Review requested from @carljm by @AlexWaygood on 2025-06-16 14:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-16 14:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-16 14:39_

---

_Label `testing` added by @AlexWaygood on 2025-06-16 14:39_

---

_Label `ty` added by @AlexWaygood on 2025-06-16 14:39_

---

_Comment by @github-actions[bot] on 2025-06-16 14:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-06-16 17:12_

This could make the tests on ci (or systems with fewer cores in general) somewhat slower because we now have to infer typeshed more often but I think this change still makes sense.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/tests/corpus.rs`:48 on 2025-06-16 17:13_

Can we make the last range open ended in case some test ends with some unicode or non alphabetic character (not high importance if we can't). The same for the start, we shouldn't exclude files starting with numbers etc.

---

_@MichaReiser approved on 2025-06-16 17:13_

---

_@AlexWaygood reviewed on 2025-06-16 17:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/tests/corpus.rs`:48 on 2025-06-16 17:26_

Oh, I just copied our existing strategy for splitting up the linter crate and tried to make it less repetitive and more declarative by using the `test_case` macro:

https://github.com/astral-sh/ruff/blob/c3aa9655468864892a23a2c2dfdd58d726b418c9/crates/ty_python_semantic/tests/corpus.rs#L42-L56

So I'm very confident that we're testing the exact same number of files on this branch that we are on `main`. But I can look into this quickly anyway

---

_@AlexWaygood reviewed on 2025-06-16 17:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/tests/corpus.rs`:48 on 2025-06-16 17:35_

fixed in https://github.com/astral-sh/ruff/pull/18711/commits/c3ccb116970de31fa8c71489dcf7baaa690bfe97

---

_Merged by @AlexWaygood on 2025-06-16 17:38_

---

_Closed by @AlexWaygood on 2025-06-16 17:38_

---

_Branch deleted on 2025-06-16 17:38_

---
