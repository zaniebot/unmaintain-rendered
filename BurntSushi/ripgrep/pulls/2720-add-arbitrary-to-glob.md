```yaml
number: 2720
title: "Add `Arbitrary` to `Glob`"
type: pull_request
state: closed
author: johnsonw
labels:
  - rollup
assignees: []
base: master
head: johnsonw/add-arbitrary-to-glob
created_at: 2024-01-24T16:19:23Z
updated_at: 2025-09-20T01:08:23Z
url: https://github.com/BurntSushi/ripgrep/pull/2720
synced_at: 2026-01-12T18:23:14Z
```

# Add `Arbitrary` to `Glob`

---

_@johnsonw_

Derive `Arbitrary` for `Glob` struct. This feature is optional so the derive will only take place when the feature is enabled. This feature is mandatory when using Glob in fuzz testing.

---

_Review comment by @BurntSushi on `crates/globset/Cargo.toml`:24 on 2024-01-24 16:22_

I don't like implicitly depending on crate names for features. Could you explicitly add an `arbitrary` feature? Example: https://github.com/rust-lang/regex/blob/0c0990399270277832fbb5b91a1fa118e6f63dba/regex-syntax/Cargo.toml#L18

---

_Review comment by @BurntSushi on `crates/globset/Cargo.toml`:24 on 2024-01-24 16:24_

Please also add docs for this feature in the crate docs, e.g., https://docs.rs/regex-syntax/latest/regex_syntax/#crate-features

---

_@BurntSushi requested changes on 2024-01-24 16:24_

Overall LGTM, with a couple changes.

Note that I am working on a rewrite of `globset` and it's not obvious to me that maintaining this `Arbitrary` impl will be straight-forward. So this might get lost in a future upgrade that will probably happen this year.

(I also resisted adding this to `regex-syntax` pretty hard, but it turned out that this was the least bad option after a lot of discussion.)

---

_Review comment by @johnsonw on `crates/globset/Cargo.toml`:24 on 2024-01-24 17:25_

Would you be ok with me adding a `fuzz` folder with a simple fuzz test?


---

_@johnsonw reviewed on 2024-01-24 17:25_

---

_@BurntSushi reviewed on 2024-01-24 17:32_

---

_Review comment by @BurntSushi on `crates/globset/Cargo.toml`:24 on 2024-01-24 17:32_

Probably fine. But it needs to be tested in CI.

---

_Marked ready for review by @johnsonw on 2024-01-24 23:45_

---

_@johnsonw reviewed on 2024-01-24 23:47_

---

_Review comment by @johnsonw on `crates/globset/Cargo.toml`:24 on 2024-01-24 23:47_

Thanks @BurntSushi. I've setup a `fuzz` directory and added a `fuzz_testing` action to ci.yml. Let me know if you want to adjust the duration for the fuzz testing as this can be modified. I've also addressed the other comments above.

---

_@jgrund reviewed on 2024-01-25 15:30_

---

_Review comment by @jgrund on `crates/globset/src/lib.rs`:22 on 2024-01-25 15:30_

```suggestion
This crate includes optional features that can be enabled if necessary. These features are
```

---

_Review requested from @BurntSushi by @johnsonw on 2024-01-25 21:38_

---

_Comment by @johnsonw on 2024-01-26 20:24_

Happy Friday @BurntSushi, I just wanted to check and see if there is anything else you would like me to address?

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:230 on 2024-01-26 21:42_

Is nightly needed here? If not, please use `@master` like the rest of the jobs.

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:228 on 2024-01-26 21:42_

Is this necessary?

---

_Review comment by @BurntSushi on `crates/globset/src/lib.rs`:22 on 2024-01-26 21:44_

Lines should be wrapped to 79 columns (inclusive).

---

_Review comment by @BurntSushi on `crates/globset/src/lib.rs`:20 on 2024-01-26 21:44_

Why is this added in the middle of an example? It should go at the bottom of the crate docs.

---

_Review comment by @BurntSushi on `fuzz/.cargo/config.toml`:4 on 2024-01-26 21:45_

Please include newlines at the end of files.

---

_Review comment by @BurntSushi on `fuzz/.cargo/config.toml`:1 on 2024-01-26 21:45_

Why does this file exist?

---

_Review comment by @BurntSushi on `fuzz/README.md`:5 on 2024-01-26 21:46_

This is one long line. It should be wrapped to 79 columns (inclusive).

---

_Review comment by @BurntSushi on `fuzz/README.md`:5 on 2024-01-26 21:46_

And the same for lines below.

---

_Review comment by @BurntSushi on `fuzz/.cargo/config.toml`:1 on 2024-01-26 21:47_

I don't think this file buys enough to warrant its existence. Having aliases for `cargo install cargo-fuzz` doesn't seem useful to me. And aliasing `cargo fuzz_list` to `cargo fuzz list` seems a little weird? Like, why?

---

_Review comment by @BurntSushi on `fuzz/fuzz_targets/fuzz_glob.rs`:4 on 2024-01-26 21:48_

I don't believe you need these two lines.

---

_Review comment by @BurntSushi on `fuzz/fuzz_targets/fuzz_glob.rs`:8 on 2024-01-26 21:49_

This should be written like this:

```rust
use std::str::FromStr;

use {globset::Glob, libfuzzer_sys::fuzz_target};
```

That is, imports should be grouped by: 1) things from std, 2) things from third party dependencies and 3) (none in this case) crate-internal imports.

---

_Review comment by @BurntSushi on `fuzz/fuzz_targets/fuzz_glob.rs`:10 on 2024-01-26 21:49_

I'd just write this as `libfuzzer_sys::fuzz_target!` and dispense with the import above.

---

_Review comment by @BurntSushi on `fuzz/rust-toolchain`:3 on 2024-01-26 21:50_

Why is this using nightly?

---

_Review comment by @BurntSushi on `fuzz/rust-toolchain`:3 on 2024-01-26 21:50_

Ideally this file wouldn't exist at all.

---

_Review comment by @BurntSushi on `fuzz/scripts/run-all.sh`:9 on 2024-01-26 21:51_

Why is this smushed on to one line? Vertical space is cheap. Use it. :)

---

_Review comment by @BurntSushi on `fuzz/scripts/run-all.sh`:1 on 2024-01-26 21:52_

The name of this script should have `fuzz` in it somewhere.

---

_Review comment by @BurntSushi on `fuzz/scripts/run-all.sh`:6 on 2024-01-26 21:53_

This should output some kind of message to stderr saying that it's going to run for `n` seconds.

---

_Review comment by @BurntSushi on `fuzz/scripts/run-all.sh`:12 on 2024-01-26 21:53_

I think overall, I would get rid of this script. It seems a little weird to be honest. I can't imagine it being used for anything really.

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:242 on 2024-01-26 21:53_

For CI, I don't think we need to actually run fuzzing. Let's just make sure all of the fuzz targets build.

---

_@BurntSushi requested changes on 2024-01-26 21:54_

Thanks. I normally take a long time to review things. So I wouldn't necessarily expect a quick turn-around here. With that said, I left a number of comments. Thank you for working on this.

---

_Review comment by @johnsonw on `.github/workflows/ci.yml`:228 on 2024-01-29 14:46_

`libFuzzer` requires LLVM sanitizer support and a C++ compiler with C++ 11 support. It is required to install the fuzzer and to compile the targets. Please see https://rust-fuzz.github.io/book/cargo-fuzz/setup.html for more information.

---

_Review comment by @johnsonw on `.github/workflows/ci.yml`:230 on 2024-01-29 14:48_

If we are not concerned with running the fuzz tests in CI then we don't need the nightly build. I'll change this to match the others.

---

_Review comment by @johnsonw on `.github/workflows/ci.yml`:242 on 2024-01-29 15:12_

Sounds good. I'll update this to simply run a `cargo check` in the `fuzz` directory.

---

_Review comment by @johnsonw on `crates/globset/src/lib.rs`:20 on 2024-01-29 16:08_

Oops, not sure how that got there :) I moved it to the bottom.

---

_Review comment by @johnsonw on `fuzz/.cargo/config.toml`:1 on 2024-01-29 16:11_

Removed.

---

_Review comment by @johnsonw on `fuzz/README.md`:5 on 2024-01-29 16:17_

Updated.

---

_Review comment by @johnsonw on `fuzz/fuzz_targets/fuzz_glob.rs`:4 on 2024-01-29 16:19_

removed.

---

_Review comment by @johnsonw on `fuzz/fuzz_targets/fuzz_glob.rs`:8 on 2024-01-29 16:21_

Updated.

---

_Review comment by @johnsonw on `fuzz/fuzz_targets/fuzz_glob.rs`:10 on 2024-01-29 16:23_

sure, updated.

---

_Review comment by @johnsonw on `fuzz/rust-toolchain`:3 on 2024-01-29 16:23_

Since it won't run in CI there is no longer a need for it. Removed.

---

_Review comment by @johnsonw on `fuzz/scripts/run-all.sh`:12 on 2024-01-29 16:26_

It's mainly used when running in CI but since the fuzz tests will not be running in CI there is no need for this file. I'll remove it.

---

_Review comment by @johnsonw on `fuzz/scripts/run-all.sh`:6 on 2024-01-29 16:27_

Script removed.

---

_Review comment by @johnsonw on `fuzz/scripts/run-all.sh`:1 on 2024-01-29 16:27_

Script removed.

---

_Review comment by @johnsonw on `fuzz/scripts/run-all.sh`:9 on 2024-01-29 16:27_

Script removed.

---

_@johnsonw reviewed on 2024-01-29 17:19_

---

_Review requested from @BurntSushi by @johnsonw on 2024-01-30 16:20_

---

_Comment by @johnsonw on 2024-02-05 17:40_

Good afternoon, I just wanted to follow up on this. Let me know if there is anything else I need to address.

---

_Comment by @BurntSushi on 2024-02-05 18:27_

I normally take a long time to review things.

---

_Label `rollup` added by @BurntSushi on 2025-07-11 19:49_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
