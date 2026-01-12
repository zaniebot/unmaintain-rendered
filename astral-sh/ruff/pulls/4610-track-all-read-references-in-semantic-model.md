```yaml
number: 4610
title: Track all read references in semantic model
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/read-references
created_at: 2023-05-23T19:31:42Z
updated_at: 2023-05-24T15:14:56Z
url: https://github.com/astral-sh/ruff/pull/4610
synced_at: 2026-01-12T03:50:03Z
```

# Track all read references in semantic model

---

_Pull request opened by @charliermarsh on 2023-05-23 19:31_

## Summary

Historically, we've only tracked the _most recent_ read reference for a given binding. To be a little more specific, we tracked the most recent "runtime", "typing-only", and "synthetic" reference (where a "synthetic" reference exists as a bit of a hack to implement always-used references, like `from foo import bar as bas`-style explicit re-exports).

For the flake8-type-checking autofix, I need to know the _first_ usage of a given symbol. And in general, we need to track all references to every symbol in order to support a class of behaviors (like symbol renaming).

This PR modifies the semantic model to support full read-reference tracking. The design is such that we store all references on the model, and each binding stores a list of IDs for its read references.

I've left the reference categorization untouched to make this a non-behavioral change, but modified the behavior to remove the "synthetic" concept in https://github.com/charliermarsh/ruff/pull/4612.

In my lazy hyperfine testing, there wasn't a noticeable regression here, but let's see what the PR benchmarks say.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-23 19:31_

---

_@charliermarsh reviewed on 2023-05-23 19:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:161 on 2023-05-23 19:31_

(I renamed this method.)

---

_@charliermarsh reviewed on 2023-05-23 19:32_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/binding.rs`:31 on 2023-05-23 19:32_

Storing the reference IDs on `Binding` gives us the nice property that testing whether something is unused remains extremely cheap.

---

_@charliermarsh reviewed on 2023-05-23 19:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:236 on 2023-05-23 19:35_

I don't really have intuition on whether we should be defining methods like `push_reference` on `SemanticModel`, or on `References` and accessing them via attribute like this.

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/reference.rs`:40 on 2023-05-23 19:44_

We can and should remove this, but it should be done separately IMO.

---

_@charliermarsh reviewed on 2023-05-23 19:44_

---

_Comment by @github-actions[bot] on 2023-05-23 19:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.6±0.76ms     2.1 MB/sec    1.01     19.7±0.51ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.15ms     3.6 MB/sec    1.03      4.7±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   576.0±20.56µs     5.1 MB/sec    1.01   580.5±25.74µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.23ms     3.2 MB/sec    1.06      8.3±0.32ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.26ms     4.4 MB/sec    1.09     10.0±0.29ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.08ms     8.3 MB/sec    1.09      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.9±11.82µs    12.1 MB/sec    1.05    254.5±9.95µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.15ms     6.0 MB/sec    1.08      4.6±0.20ms     5.6 MB/sec
parser/large/dataset.py                    1.02      7.3±0.20ms     5.5 MB/sec    1.00      7.2±0.15ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.03  1467.2±53.46µs    11.3 MB/sec    1.00  1428.3±70.24µs    11.7 MB/sec
parser/numpy/globals.py                    1.04    142.6±7.92µs    20.7 MB/sec    1.00    137.1±5.75µs    21.5 MB/sec
parser/pydantic/types.py                   1.02      3.2±0.09ms     8.1 MB/sec    1.00      3.1±0.10ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.4±0.28ms  1943.8 KB/sec    1.00     21.2±0.36ms  1961.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.12ms     3.1 MB/sec    1.00      5.3±0.13ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   629.4±19.44µs     4.7 MB/sec    1.00   630.4±24.74µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.21ms     2.8 MB/sec    1.00      8.9±0.22ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.4±0.18ms     3.9 MB/sec    1.00     10.2±0.20ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.15ms     7.4 MB/sec    1.00      2.2±0.06ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    259.5±8.99µs    11.4 MB/sec    1.03   268.3±14.56µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.15ms     5.4 MB/sec    1.00      4.7±0.13ms     5.4 MB/sec
parser/large/dataset.py                    1.01      8.0±0.16ms     5.1 MB/sec    1.00      7.9±0.11ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1516.7±37.89µs    11.0 MB/sec    1.00  1511.4±39.73µs    11.0 MB/sec
parser/numpy/globals.py                    1.01    154.8±7.78µs    19.1 MB/sec    1.00    153.1±5.23µs    19.3 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.08ms     7.5 MB/sec    1.00      3.4±0.09ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-23 20:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/reference.rs`:40 on 2023-05-23 20:26_

See: https://github.com/charliermarsh/ruff/pull/4612

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:236 on 2023-05-23 21:39_

Creating a method on the semantic model makes the call shorter and allows to default to the current scope (so that you don't need to specify current scope everywhere)

I generally prefer to encapsulate all mutations, to have better control over how the data structures are modified (and only allow the operations that are valid)

---

_@MichaReiser reviewed on 2023-05-23 21:39_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:305 on 2023-05-23 21:41_

Same here: semantic_model.reference(binding_id, reference_id) is shorter, less to read in the future. It also hides how references are stored internally, making it easier to experiment with different representations 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4406 on 2023-05-23 21:44_

Nit: model.is_used(binding)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4411 on 2023-05-23 21:45_

Do we have to clone here. It seems we're moving all other data

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:68 on 2023-05-23 21:47_

Nit: binding.references(). Makes it shorter and hides implementation details



---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/reference.rs`:72 on 2023-05-23 21:50_

You should be able to derive the default implementation 

---

_@MichaReiser reviewed on 2023-05-23 21:51_

Nice. We may want to look into ways that we can speed this up (smallvec?) because it seems to affect the performance significantly 

---

_Comment by @charliermarsh on 2023-05-23 22:11_

I'm game to try speeding this up, but I am a little skeptical that the difference is so great in practice. In my hyperfine benchmarks (which are less precise, but in some ways (?) more representative), there's basically no difference:

```
❯ cargo build --release && hyperfine --warmup 10 --runs 100 "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e" && cargo build --release && hyperfine --warmup 10 --runs 100 "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e" && git co main && cargo build --release && hyperfine --warmup 10 --runs 100 "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e" && cargo build --release && hyperfine --warmup 10 --runs 100 "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e"
    Finished release [optimized] target(s) in 0.20s
Benchmark 1: ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     228.6 ms ±   4.5 ms    [User: 1795.1 ms, System: 68.5 ms]
  Range (min … max):   222.5 ms … 255.6 ms    100 runs

    Finished release [optimized] target(s) in 0.13s
Benchmark 1: ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     230.3 ms ±   4.6 ms    [User: 1804.0 ms, System: 68.4 ms]
  Range (min … max):   223.7 ms … 251.9 ms    100 runs

Switched to branch 'main'
Your branch is up to date with 'origin/main'.
   Compiling ruff_python_semantic v0.0.0 (/Users/crmarsh/workspace/staging/crates/ruff_python_semantic)
   Compiling ruff v0.0.269 (/Users/crmarsh/workspace/staging/crates/ruff)
   Compiling ruff_cli v0.0.269 (/Users/crmarsh/workspace/staging/crates/ruff_cli)
   Compiling ruff_dev v0.0.0 (/Users/crmarsh/workspace/staging/crates/ruff_dev)
   Compiling flake8-to-ruff v0.0.269 (/Users/crmarsh/workspace/staging/crates/flake8_to_ruff)
   Compiling ruff_wasm v0.0.0 (/Users/crmarsh/workspace/staging/crates/ruff_wasm)
    Finished release [optimized] target(s) in 1m 50s
Benchmark 1: ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     236.7 ms ±  12.1 ms    [User: 1773.4 ms, System: 70.6 ms]
  Range (min … max):   220.5 ms … 283.3 ms    100 runs

    Finished release [optimized] target(s) in 0.15s
Benchmark 1: ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache --silent -e
  Time (mean ± σ):     229.1 ms ±   7.0 ms    [User: 1805.6 ms, System: 72.1 ms]
  Range (min … max):   221.0 ms … 267.1 ms    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
```

---

_@charliermarsh reviewed on 2023-05-23 22:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4411 on 2023-05-23 22:15_

The fields on `existing` need to be cloned, yeah. The fields on `binding` are moved.

---

_Comment by @charliermarsh on 2023-05-23 22:22_

(Great feedback, thank you.)

---

_@charliermarsh reviewed on 2023-05-23 22:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:305 on 2023-05-23 22:35_

I simplified to `self.semantic_model.add_local_reference`, which creates and adds the reference.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/helpers.rs`:60 on 2023-05-24 07:19_

Nit: It could make sense to add `is_*` methods is this is something we commonly test for
```suggestion
                
                semantic_model.references.resolve(reference_id).context().is_runtime()
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:67 on 2023-05-24 07:20_

Same for the `ExecutionContext`: 
```suggestion
    if binding.context.is_typing()
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/rules/undefined_local.rs`:51 on 2023-05-24 07:22_

Nit and for another PR: Is `semantic_model().scope_id` the id of the current scope? Should we add a `is_current_scoipe(scope_id)` method or add a `model.current_scope_id()`

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:135 on 2023-05-24 07:23_

Nit Nit: `context.invert()`?

---

_@MichaReiser approved on 2023-05-24 07:23_

> I'm game to try speeding this up, but I am a little skeptical that the difference is so great in practice. In my hyperfine benchmarks (which are less precise, but in some ways (?) more representative), there's basically no difference:

Yeah, that's fair. A few thoughts:

* I prefer building both release binaries and copying them to a dedicated location. Then run both hyperfine commands at once, to provide an as identical environment as possible to both benchmark runs (e.g. `cargo build` is very performance intense, running the benchmark right after may mean that the CPUs already thermal throttled)
* The hyperfine benchmark is important to make "meaningful" decisions, but it also includes a lot of noise and is probably mainly limited by IO. I think it's worth spending some time optimizing our implementations if the microbenchmarks show a regression because performance dies by a thousand cuts. We may reach a point in the future where ruff suddenly is CPU limited, identifying all performance optimisations then will be much harder then when doing them right away.
* Regressing performance is fine if we have a clear reason why the feature is needed. 

It may also be that the benchmarks on the CI are just flaky. I recommend you to run them locally on main (use `--save-baselne main`)  and then on your branch using `--baseline main`. You can use the time two tweet something nice on your phone or get a coffee or two ;)

---

_Comment by @charliermarsh on 2023-05-24 13:56_

I'll use the benchmarking time to tweet something nice about Micha!

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:135 on 2023-05-24 14:08_

I'm going to do this in a separate PR, because these can be the same type once we remove `Synthetic`.

---

_@charliermarsh reviewed on 2023-05-24 14:08_

---

_Merged by @charliermarsh on 2023-05-24 14:14_

---

_Closed by @charliermarsh on 2023-05-24 14:14_

---

_Branch deleted on 2023-05-24 14:14_

---
