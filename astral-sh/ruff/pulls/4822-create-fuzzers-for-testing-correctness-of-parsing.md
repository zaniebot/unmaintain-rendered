```yaml
number: 4822
title: Create fuzzers for testing correctness of parsing, linting and fixing
type: pull_request
state: merged
author: addisoncrump
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2023-06-03T03:54:36Z
updated_at: 2023-06-07T13:08:20Z
url: https://github.com/astral-sh/ruff/pull/4822
synced_at: 2026-01-12T03:43:29Z
```

# Create fuzzers for testing correctness of parsing, linting and fixing

---

_Pull request opened by @addisoncrump on 2023-06-03 03:54_

## Summary

This PR introduces multiple fuzzers which test the correctness of Ruff. Namely:

- `ruff_parse_simple`, which attempts to simply crash the parser (though is mainly useful as a utility for generating inputs)
- `ruff_parse_idempotency`, which searches for idempotency violations in the parse/unparse utilities of Ruff
- `ruff_fix_validity`, which checks that fixes applied by Ruff do not introduce syntax violations (using `ruff::test::test_snippet`)

## Test Plan

This introduces new tests. I will open PRs with a few of the bugs discovered by the fuzzer and link them to this PR to demonstrate some of the things it is able to find.

---

_Comment by @github-actions[bot] on 2023-06-03 04:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.02ms     6.6 MB/sec    1.19      7.3±0.02ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1257.7±9.70µs    13.2 MB/sec    1.14   1439.6±2.26µs    11.6 MB/sec
formatter/numpy/globals.py                 1.00    145.1±1.20µs    20.3 MB/sec    1.09    157.8±0.74µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.4 MB/sec    1.16      3.1±0.03ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.02     15.0±0.12ms     2.7 MB/sec    1.00     14.7±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    364.7±0.89µs     8.1 MB/sec    1.00    362.4±0.82µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.03ms     5.5 MB/sec    1.00      7.3±0.09ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1535.2±3.31µs    10.8 MB/sec    1.01   1550.2±3.51µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±0.16µs    17.9 MB/sec    1.01    165.6±0.61µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.1±0.18ms     6.6 MB/sec    1.00      6.1±0.08ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1232.2±44.55µs    13.5 MB/sec    1.01  1250.1±48.50µs    13.3 MB/sec
formatter/numpy/globals.py                 1.00    138.4±2.74µs    21.3 MB/sec    1.03    142.3±3.75µs    20.7 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.05ms     9.5 MB/sec    1.01      2.7±0.07ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.01     14.6±0.15ms     2.8 MB/sec    1.00     14.5±0.12ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.04ms     4.5 MB/sec    1.00      3.7±0.04ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   435.5±11.61µs     6.8 MB/sec    1.00    434.4±6.63µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.07ms     4.1 MB/sec    1.00      6.2±0.14ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.06ms     5.6 MB/sec    1.01      7.3±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1531.6±23.47µs    10.9 MB/sec    1.01  1542.7±25.13µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.5±4.46µs    16.9 MB/sec    1.00    174.8±6.41µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.01      3.3±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-06-03 05:06_

This looks pretty cool! I'm looking forward to seeing this uncover more bugs — the two you linked already are nice finds.

---

_Comment by @addisoncrump on 2023-06-03 05:16_

> This looks pretty cool! I'm looking forward to seeing this uncover more bugs — the two you linked already are nice finds.

Thanks! I hope it gets some good use -- manually sifting through the bugs right now to deduplicate, but hopefully once all the low-hanging fruit issues are out of the way we can integrate it into the standard testing process. :grin: 

---

_Comment by @addisoncrump on 2023-06-03 05:19_

Also, it should be fairly straightforward to extend the idempotency ideas to #4798 for testing the formatter as well, when this is completed.

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:98 on 2023-06-05 06:32_

Does `cargo-bininstall` support `cargo-fuzz`? It could help to speed up the CI step

https://github.com/charliermarsh/ruff/blob/466719247bc68c0b608bc85be341c5aa5e2a5ec2/.github/workflows/benchmark.yaml#L81-L85

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-05 06:36_

This seems a bit painfull. Should we extract the logic of `test_path` into a `TestRunner` struct that can be configured:

```rust
pub struct TestRunner<'a> {
	path: &'a Path
	settings: &'a Settings,
	iterations: usize
}

impl<'a> TestRunner<'a> {
	pub fn new(path: &'a Path, settings: &'a Settings) -> Self {
		Self { path, settings, iterations: DEFAULT_MAX_ITERATIONS }
	}

	pub fn with_iterations(mut self, iterations: usize) -> Self {
		self.iterations = iterations;
		self
	}

	pub fn run(self) -> Diagnostics {
		// All the test_path code
	}
}
```

Another option would be to create a `TestRunner` struct that accepts various options with a single `run` method.  `test_path` could use that struct internally and you could configure it.


Nit: For the future: It may even be worth to accept settings as an owned value so that it becomes possible to create a `for_rule` factory method that creates the settings using `settings::Settings::for_rule(rule_code)` internally. It seems silly, that we repeat this for every rule.

---

_Review comment by @MichaReiser on `crates/ruff/src/test.rs`:54 on 2023-06-05 06:39_

Same as for `test_path`. We should extract the logic into a `TestContentsRunner` and use it inside of `test_contents`. This way, it becomes possible for you to use the advanced `runner` API without having to change all call sites (and reduces the number of arguments).

---

_Review comment by @MichaReiser on `crates/ruff/src/test.rs`:146 on 2023-06-05 06:40_

Is this change intentional?

---

_Review comment by @MichaReiser on `fuzz/Cargo.toml`:9 on 2023-06-05 06:41_

Nit: You can inherit the edition from the workspace
```suggestion
edition = { workspace = true }
```

---

_Review comment by @MichaReiser on `fuzz/Cargo.toml`:7 on 2023-06-05 06:42_

CC: @charliermarsh 

---

_Review comment by @MichaReiser on `fuzz/Cargo.toml`:23 on 2023-06-05 06:43_

I see. This is not part of the workspace. What's the motivation for not making this crate part of the Ruff workspace? It being its own workspace increases makes versioning harder (we now need to keep in mind to update rust python here and in the main ruff workspace)

---

_Review comment by @MichaReiser on `fuzz/Cargo.toml`:52 on 2023-06-05 06:44_

What's the motivation for opting in to opt-level 3 even for debug? Would a lower level like 1 or 2 suffice?

---

_Review comment by @MichaReiser on `fuzz/README.md`:3 on 2023-06-05 06:45_

This Readme is awesome.

---

_Review comment by @MichaReiser on `fuzz/README.md`:16 on 2023-06-05 06:47_

How can I skip the dataset download or does the script ask me whether to download the dataset?

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_fix_validity.rs`:9 on 2023-06-05 06:47_

You can use `OnceCell` for a lazy initialized static object without relying on unsafe code https://doc.rust-lang.org/std/cell/struct.OnceCell.html

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_fix_validity.rs`:21 on 2023-06-05 06:48_

This could, potentially, result in an infinite loop if two ruff fixes don't play nicely together. 

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_parse_idempotency.rs`:45 on 2023-06-05 06:49_

Idea: You could use similar to produce a diff

https://github.com/charliermarsh/ruff/blob/466719247bc68c0b608bc85be341c5aa5e2a5ec2/crates/ruff_python_formatter/src/lib.rs#L212-L215

---

_Review comment by @MichaReiser on `fuzz/init-fuzzer.sh`:10 on 2023-06-05 06:50_

Should we ask users to install `cargo-fuzz` themselves? 

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:101 on 2023-06-05 06:52_

Improvement (doesn't have to be part of this PR and we face the same problem with benchmarks). It would be great if we could run the fuzzer selectively 

* parser tests: Only when the `Cargo.toml` changes (as an over approximation for RustPython changed)
* linter: changes to any non-formatter or cli related crate?

---

_@MichaReiser reviewed on 2023-06-05 06:53_

This is awesome and very well done. Thank you so much. Also thank you for filling some issues already. 

I've a few comments but overall looking good.

---

_@addisoncrump reviewed on 2023-06-05 11:22_

---

_Review comment by @addisoncrump on `crates/ruff/src/test.rs`:146 on 2023-06-05 11:22_

Nope! Sorry, must have had it in my clipboard :sweat: 

---

_Review comment by @addisoncrump on `fuzz/Cargo.toml`:9 on 2023-06-05 11:22_

Actually, not so. Fuzz needs to be in its own workspace and cannot import from the parent workspace, as the build uses separate RUSTFLAGS.

---

_@addisoncrump reviewed on 2023-06-05 11:22_

---

_@addisoncrump reviewed on 2023-06-05 11:23_

---

_Review comment by @addisoncrump on `fuzz/README.md`:16 on 2023-06-05 11:23_

The script asks :slightly_smiling_face: 

---

_@addisoncrump reviewed on 2023-06-05 11:23_

---

_Review comment by @addisoncrump on `fuzz/Cargo.toml`:23 on 2023-06-05 11:23_

See previous answer :slightly_smiling_face: 

---

_@addisoncrump reviewed on 2023-06-05 11:26_

---

_Review comment by @addisoncrump on `fuzz/Cargo.toml`:52 on 2023-06-05 11:26_

Fuzzers rely on being able to execute quickly, and (if unsafe code is involved) some faults are not consistent between optimisation levels. Having these be as close as possible is important. Comparatively, you can build the fuzzers on their own (just `cargo run ...` instead of `cargo fuzz run ...`) if you want to analyse a crash without [fuzzer instrumentation](https://clang.llvm.org/docs/SanitizerCoverage.html) (in some rare cases, the instrumentation can modify behaviour or optimisation). This is a rare use case but something I wanted to allow for.

---

_@addisoncrump reviewed on 2023-06-05 11:27_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_fix_validity.rs`:21 on 2023-06-05 11:27_

This would be detected as a timeout and the fuzzer would mark it as a crash, so this is actually okay :slightly_smiling_face: 

---

_@addisoncrump reviewed on 2023-06-05 11:28_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_fix_validity.rs`:9 on 2023-06-05 11:28_

Ah, okay. Wasn't sure of the performance implications here, but I'll give it a try.

---

_@addisoncrump reviewed on 2023-06-05 11:30_

---

_Review comment by @addisoncrump on `fuzz/init-fuzzer.sh`:10 on 2023-06-05 11:30_

Potentially; however, cargo fuzz recommends installing via `cargo install cargo-fuzz`, but their release cycle is very slow and the main branch is normally stable. For having all potential features available, it's better to install via the git and I figured it would be easier to do so in a script.

---

_@addisoncrump reviewed on 2023-06-05 11:30_

---

_Review comment by @addisoncrump on `fuzz/Cargo.toml`:7 on 2023-06-05 11:30_

I leave authorship details to your discretion; this was autofilled by my IDE.

---

_@addisoncrump reviewed on 2023-06-05 11:33_

---

_Review comment by @addisoncrump on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-05 11:33_

I do notice that the `MAX_ITERATIONS` paradigm seems to be consistent throughout the code base, and normally set by a fixed global value. Perhaps this should just be configurable in general. I made the "simplest" changes for proof-of-concept, so I leave this to your discretion on how this should be implemented as it will dramatically affect all testing code.

---

_Review comment by @addisoncrump on `.github/workflows/ci.yaml`:101 on 2023-06-05 11:36_

Definitely would be cool to run the fuzzers for a short amount of time! To do so, we'd need the following in CI:
 - caching of the fuzzer corpus
 - 10 minute fuzz runs (maybe only on merges to main/release pre-checks? potentially would need a merge queue)

That said, Ruff isn't quite resistant enough to fuzzing yet to justify adding it to the CI. Probably all your builds would "break" due to the fuzzer finding things that simply haven't been fixed yet. I haven't even properly confirmed that I've found "all" the potential issues yet.

---

_@addisoncrump reviewed on 2023-06-05 11:36_

---

_@addisoncrump reviewed on 2023-06-05 11:37_

---

_Review comment by @addisoncrump on `.github/workflows/ci.yaml`:98 on 2023-06-05 11:37_

I am unfamiliar with bininstall. I will try this :slightly_smiling_face: 

---

_@addisoncrump reviewed on 2023-06-05 13:04_

---

_Review comment by @addisoncrump on `.github/workflows/ci.yaml`:98 on 2023-06-05 13:04_

Hm, looking at the CI, it seems that none of the other installations in `ci.yaml` use binstall. Perhaps we should make this a separate PR instead.

---

_@addisoncrump reviewed on 2023-06-05 13:23_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_parse_idempotency.rs`:45 on 2023-06-05 13:23_

I tried this, but the results were not good. A lot of the fuzz cases are very malformed before minimisation. Here's an example of the "diff" output:

```diff
diff:
--- Parsed twice
+++ Parsed three times
@@ -1 +1 @@
-b % f"{7:}{7::\\x00\\x00\\x00R\\x00:}{7:\\x00\\x00\\x00R\\x00R\\x00:}{7::\\x00\\x00\\x00:}{4:\\x00\\x00\\x00:\\x00\\x00\\x00\\x00\\x00R\\x00:}{7::\\x00\\x00\\x00R\\x00:}{7:\\x00\\x00\\x00R\\x00R\\x00:}{7::\\x00\\x00\\x00:}{4:\\x00\\x00\\x00R\\x00\\x00\\x00R\\x00:}8"
\ No newline at end of file
+b % f"{7:}{7::\\\\x00\\\\x00\\\\x00R\\\\x00:}{7:\\\\x00\\\\x00\\\\x00R\\\\x00R\\\\x00:}{7::\\\\x00\\\\x00\\\\x00:}{4:\\\\x00\\\\x00\\\\x00:\\\\x00\\\\x00\\\\x00\\\\x00\\\\x00R\\\\x00:}{7::\\\\x00\\\\x00\\\\x00R\\\\x00:}{7:\\\\x00\\\\x00\\\\x00R\\\\x00R\\\\x00:}{7::\\\\x00\\\\x00\\\\x00:}{4:\\\\x00\\\\x00\\\\x00R\\\\x00\\\\x00\\\\x00R\\\\x00:}8"
\ No newline at end of file
```

~~Unfortunately, the diff is not nearly as helpful as viewing the samples individually _after_ minimising the testcase.~~ Actually, it seems that the diff is actually okay after minimisation:

```
thread '<unnamed>' panicked at 'assertion failed: `(left == right)`
  left: `"f\"{7:\\\\x00}\""`,
 right: `"f\"{7:\\\\\\\\x00}\""`: 
Potential unsteady state (orig => first => second => third); original: "f\"{7:\0}\""
diff:
--- Parsed twice
+++ Parsed three times
@@ -1 +1 @@
-f"{7:\\x00}"
\ No newline at end of file
+f"{7:\\\\x00}"
\ No newline at end of file
', fuzz_targets/ruff_parse_idempotency.rs:34:17
```

---

_@addisoncrump reviewed on 2023-06-05 15:58_

---

_Review comment by @addisoncrump on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-05 15:58_

Hm, yeah, potentially I'll need to shift away from the test runner code anyways, as I need to control for undesired testcases (see: https://github.com/charliermarsh/ruff/issues/4865).

---

_Review comment by @MichaReiser on `fuzz/Cargo.toml`:23 on 2023-06-05 17:21_

Oh I see. That makes sense. Would it be possible to use the re-exported nodes from `ruff_python_ast` (`prelude.rs`)? 

---

_@MichaReiser reviewed on 2023-06-05 17:21_

---

_@addisoncrump reviewed on 2023-06-05 17:23_

---

_Review comment by @addisoncrump on `fuzz/Cargo.toml`:23 on 2023-06-05 17:23_

Should be able to, yes -- let me try this.

---

_@addisoncrump reviewed on 2023-06-05 17:29_

---

_Review comment by @addisoncrump on `fuzz/Cargo.toml`:23 on 2023-06-05 17:29_

Turns out I didn't even use these deps :upside_down_face: 

---

_@charliermarsh reviewed on 2023-06-05 20:28_

This is impressive work.

---

_@charliermarsh reviewed on 2023-06-05 20:44_

---

_Review comment by @charliermarsh on `fuzz/Cargo.toml`:7 on 2023-06-05 20:44_

The authors field is ["open to interpretation"](https://doc.rust-lang.org/cargo/reference/manifest.html#the-authors-field) but I mostly see it as a mechanism through which users can contact maintainers. Can you add me as well here (`"Charlie Marsh <charlie.r.marsh@gmail.com>"`)? Since I'll be the primarily-responsible maintainer (or at least the point of contact) in the future.

---

_@charliermarsh reviewed on 2023-06-05 20:46_

---

_Review comment by @charliermarsh on `fuzz/Cargo.toml`:7 on 2023-06-05 20:46_

(Feel free to include yourself here or remove as you prefer, I don't feel strongly, I'd just like to have myself listed in addition as the primary maintainer.)

---

_Comment by @addisoncrump on 2023-06-06 10:10_

This last commit adds some sugar so I can fuzz locally with libafl (disclosure: this is a fuzzer I am a maintainer of), which is orders of magnitude faster than libfuzzer but has less support. It's not default and shouldn't affect normal users.

---

_Comment by @jvoisin on 2023-06-06 12:02_

It would be glorious to add this to [OSS-Fuzz]( https://github.com/google/oss-fuzz ) <3

---

_Comment by @addisoncrump on 2023-06-06 12:04_

That's the plan :wink: 

---

_@addisoncrump reviewed on 2023-06-06 12:59_

---

_Review comment by @addisoncrump on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-06 12:59_

Quick bump on this; what are the action items here?

---

_@MichaReiser reviewed on 2023-06-06 15:33_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:98 on 2023-06-06 15:33_

Good point. They probably should. But I understand we want to remove the CI step for now anyway because there would be too many false positives. I recommend either dropping the CI step or adding it in its own PR to resolve the conversation for now.

---

_@MichaReiser reviewed on 2023-06-06 15:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-06 15:34_

I think it depends on the direction you want to take. Do you plan to migrate away from the test runner code?

---

_Review comment by @addisoncrump on `.github/workflows/ci.yaml`:98 on 2023-06-06 15:35_

We want to keep the build step. In the future, we should add the fuzz runs as another CI step as well.

---

_@addisoncrump reviewed on 2023-06-06 15:35_

---

_@addisoncrump reviewed on 2023-06-06 15:36_

---

_Review comment by @addisoncrump on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-06 15:36_

Not anymore -- realised the issue was actually resolvable without moving away from the test runner. In reality, we want fuzzing to stay as close to testing code as possible -- and allow you to update it as seen fit.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/airflow/mod.rs`:21 on 2023-06-06 15:41_

An alternative (that doesn't require changing all tests) is to provide an API to configure the max iterations value:

```rust
thread_local! {
    static MAX_ITERATIONS: std::cell::Cell<u32> = std::cell::Cell::new(1);
}

pub fn set_max_iterations(max: u32) {
    MAX_ITERATIONS.with(|iterations| iterations.set(max))
}

pub(crate) fn max_iterations() -> u32 {
    MAX_ITERATIONS.with(|iterations| iterations.get())
}
```

I would, ultimately prefer a `TestRunner` API but I can see how this is out of scope for this PR. Would that thread local state work for you?

---

_@MichaReiser approved on 2023-06-06 15:42_

---

_Comment by @addisoncrump on 2023-06-06 15:44_

Potentially, yes :slightly_smiling_face: Though, this seems really roundabout. Why does this need to be a global/constant? Should this perhaps be a Setting entry? This pattern also appears here: https://github.com/charliermarsh/ruff/blob/1ed5d7e437a1dda0f4c2ddae09f5a7aa7d713bde/crates/ruff/src/linter.rs#L247

---

_Comment by @MichaReiser on 2023-06-06 16:53_

> Potentially, yes slightly_smiling_face Though, this seems really roundabout. Why does this need to be a global/constant? Should this perhaps be a Setting entry? This pattern also appears here:
> 
> https://github.com/charliermarsh/ruff/blob/1ed5d7e437a1dda0f4c2ddae09f5a7aa7d713bde/crates/ruff/src/linter.rs#L247

The settings is what we use in production. I would prefer to keep max iterations local to the testing infrastructure. 

---

_Comment by @addisoncrump on 2023-06-06 16:56_

I see. This would work in my case, yes. Applying the change!

---

_Label `internal` added by @MichaReiser on 2023-06-07 12:57_

---

_Merged by @MichaReiser on 2023-06-07 12:57_

---

_Closed by @MichaReiser on 2023-06-07 12:57_

---
