```yaml
number: 17171
title: Upgrade to Rust 1.86 and bump MSRV to 1.84
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-1.86
created_at: 2025-04-03T11:44:33Z
updated_at: 2025-05-07T10:37:21Z
url: https://github.com/astral-sh/ruff/pull/17171
synced_at: 2026-01-10T18:57:02Z
```

# Upgrade to Rust 1.86 and bump MSRV to 1.84

---

_Pull request opened by @MichaReiser on 2025-04-03 11:44_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I decided to disable the new [`needless_continue`](https://rust-lang.github.io/rust-clippy/master/index.html#needless_continue) rule because I often found the explicit `continue` more readable over an empty block or having to invert the condition of an other branch. 


## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2025-04-03 11:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-03 11:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `internal` added by @MichaReiser on 2025-04-03 12:37_

---

_Marked ready for review by @MichaReiser on 2025-04-03 12:45_

---

_Review requested from @carljm by @MichaReiser on 2025-04-03 12:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-03 12:45_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-03 12:45_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-03 12:45_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-04-03 12:45_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-03 12:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/typeshed.rs`:337 on 2025-04-03 12:46_

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:983 on 2025-04-03 12:48_

if we expect this `write!()` call to always succeed, would `.unwrap()` or `.expect()` be better?

---

_@AlexWaygood approved on 2025-04-03 12:49_

---

_@MichaReiser reviewed on 2025-04-03 12:52_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:983 on 2025-04-03 12:52_

I thought about that and it actually was the first thing I wrote (there was no autofix). 

I went with the lint's recommended fix because I expect most users to do the same, and this approach will lead to more consistent code. 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_gettext/rules/printf_in_gettext_func_call.rs`:57 on 2025-04-03 14:08_

Interesting, is this now allowed or was it always allowed and I wasn't aware of it? i.e., matching against an enum struct variant without specifying the fields.

---

_@dhruvmanila approved on 2025-04-03 14:09_

---

_@MichaReiser reviewed on 2025-04-03 15:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_gettext/rules/printf_in_gettext_func_call.rs`:57 on 2025-04-03 15:55_

`Operator::Mod` has no fields. But it seems that Rust still allows you to use `{ .. } ` in that case because that also matches "no-fields"

---

_Merged by @MichaReiser on 2025-04-03 15:59_

---

_Closed by @MichaReiser on 2025-04-03 15:59_

---

_Branch deleted on 2025-04-03 15:59_

---

_@dhruvmanila reviewed on 2025-04-03 16:01_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_gettext/rules/printf_in_gettext_func_call.rs`:57 on 2025-04-03 16:01_

I see and I'm guessing that it's the same for other struct variants for which the `{ .. }` was removed in this PR.

---

_Review comment by @BurntSushi on `crates/ruff/src/commands/rule.rs`:56 on 2025-04-03 18:43_

I kinda wonder if we should propose add something to `std` to make this nicer. I like the way the `x.push_str(&format!("..."))` construction reads much nicer, but I guess this approach avoids an intermediate allocation. But this approach has the very ugly `let _ = ...;` construction.

---

_@BurntSushi reviewed on 2025-04-03 18:43_

Nice! Sorry for the late review. I was in the middle of reviewing this when I scratched my cornea, so I figured I'd finish it haha.

---

_@MichaReiser reviewed on 2025-04-03 18:49_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/rule.rs`:56 on 2025-04-03 18:49_

That would be great. I definitely don't love it and it has the downside that later refactoring the code to eg use a Write instead of a string now incorrectly suppresses errors (instead of Rust telling you that this might now fail)

---

_Comment by @eskultety on 2025-05-07 08:47_

Hello, I'd like to know what the motivation to pin the Rust version to the latest release in `rust-toolchain.toml` was? Were there any interesting features that this project required to adopt that quickly (literally on the same day of Rust release)?
The reason for my question is that IIUC you're essentially making it impossible for any distro out there to keep up with packaging and everyone is required to rely on `rustup` to get the exact toolchain version needed to build the project. Before I dive into any further discussion I'd first like to know where my lack of understanding the matter resides and therefore what the real motivation here was/is.

---

_Comment by @MichaReiser on 2025-05-07 09:11_

> Hello, I'd like to know what the motivation to pin the Rust version to the latest release in `rust-toolchain.toml` was? Were there any interesting features that this project required to adopt that quickly (literally on the same day of Rust release)? The reason for my question is that IIUC you're essentially making it impossible for any distro out there to keep up with packaging and everyone is required to rely on `rustup` to get the exact toolchain version needed to build the project. Before I dive into any further discussion I'd first like to know where my lack of understanding the matter resides and therefore what the real motivation here was/is.

We're aware that always using the latest version is challenging for downstream packagers. That's why we only use the latest Rust version for local development (and in our release pipelines) but maintain backwards compatibility with `latest - 2` as the MSRV with which ruff compiles (see [versioning policy](https://docs.astral.sh/ruff/versioning/#minimum-supported-rust-version)). That means Ruff can still be built with Rust 1.84 today.

I think what you're running into is that you clone Ruff's repository and invoke `cargo build` directly. Cargo then picks up the version specified in `rust-toolchain.toml`. The version in `rust-toolchain.toml` is the version *we use* and using the latest Rust version is desired because it gives us new clippy rules and enables new compiler optimizations in the release artifacts built as part of our pipeline. 

I understand is that you want to use a different Rust version. You have a few options:

* use `rustup override set <version>`
* Use a toolchain override shorthand `cargo +stable`
* Use the `RUSTUP_TOOLCHAIN`
* Delete the `rust-toolchain.toml`


You can use any rust version that's compatible with our MSRV (1.84 today). It can be newer or older than the version in our `rust-toolchain.toml` but it can't be older than the MSRV.

I hope this helps



---

_Comment by @eskultety on 2025-05-07 09:33_

> That's why we only use the latest Rust version for local development (and in our release pipelines)

Hmm, have you considered adopting the latest & greatest primarily in nightly CI builds to get early feedback on new stuff but move the upstream dependency on a slower pace to essentially not "test in production" and by production means all local dev setups? Kinda what Fedora Rawhide is for and commonly consumed in projects' CIs.

> I think what you're running into is that you clone Ruff's repository and invoke cargo build directly. Cargo then picks up the version specified in rust-toolchain.toml

Our use case is actually quite different (ties to `cargo vendor`, not important), but yes, the result is the same. We work in the secure software supply chain field with a strong compliance with the strictest [SLSA](https://slsa.dev/spec/v1.0/) levels which, among other things, means that builds are network-isolated, hence no rustup updates (also no rustup package). The other major factor (as if network isolation weren't enough :) ) is strict provenance, to put it simply - consuming packages via source tarballs is preferred over binaries.

> Use a toolchain override shorthand cargo +stable
> Use the RUSTUP_TOOLCHAIN
> Delete the rust-toolchain.toml

We may actually consider some of ^this, thanks for the pointer, I admit I didn't pay thorough attention when reading https://rust-lang.github.io/rustup/overrides.html before dropping a comment on this PR. That said, if you say that `rust-toolchain.toml` is for development and to consume optimizations early for your binary artifacts, would you consider excluding the file from the Python sdist since based on the premise (and based on your backwards compatibility claim) it's irrelevant to have it there for released packages.

---

_Comment by @MichaReiser on 2025-05-07 09:59_

> That said, if you say that rust-toolchain.toml is for development and to consume optimizations early for your binary artifacts, would you consider excluding the file from the Python sdist since based on the premise (and based on your backwards compatibility claim) it's irrelevant to have it there for released packages.

I think that makes sense

---

_Comment by @MichaReiser on 2025-05-07 10:37_

I created https://github.com/astral-sh/ruff/issues/17909. I'm trying to figure out why we included it in the first place.

---
