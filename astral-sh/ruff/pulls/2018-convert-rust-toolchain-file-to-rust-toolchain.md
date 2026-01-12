```yaml
number: 2018
title: Convert rust-toolchain file to rust-toolchain.toml (with nightly)
type: pull_request
state: closed
author: akx
labels: []
assignees: []
base: main
head: toolchain-file
created_at: 2023-01-20T09:45:53Z
updated_at: 2023-01-20T10:50:24Z
url: https://github.com/astral-sh/ruff/pull/2018
synced_at: 2026-01-12T04:52:00Z
```

# Convert rust-toolchain file to rust-toolchain.toml (with nightly)

---

_Pull request opened by @akx on 2023-01-20 09:45_

This makes IDEA IDEs realize they should use `nightly` as documented in `CONTRIBUTING.md`.

This will require rustup 1.23.0+ (which was released in 2020, so likely a non-issue).

Refs https://github.com/intellij-rust/intellij-rust/issues/6228
Refs https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file

---

_Comment by @not-my-profile on 2023-01-20 09:49_

The `rust-toolchain` file previously contained `1.65.0` and you changed it to `nightly` ... this is not the same.

---

_Comment by @akx on 2023-01-20 09:49_

@not-my-profile I know I did. `CONTRIBUTING.md` clearly says to use `+nightly` too (and indeed, one of the `rust_dev` deps doesn't compile on non-nightly).

---

_Comment by @not-my-profile on 2023-01-20 09:56_

Ruff does not depend on a single unstable feature so we should continue to have `cargo test` use stable Rust. I only use `+nigthly` for `cargo fmt` and everything else works fine on stable.

>and indeed, one of the rust_dev deps doesn't compile on non-nightly

`rust_dev` works fine on stable for me.

In any case I think a config format change and a version change are two separate changes and should certainly be done in separate commits. And a version change should have some convincing reasoning (ideally also explained in the commit message).

---

_Comment by @akx on 2023-01-20 10:01_

@not-my-profile 

> we should continue to have `cargo test` use stable Rust.

It looks like CI runs tests on nightly, unless I'm mistaken and there's some additional override? https://github.com/charliermarsh/ruff/blob/81db00a3c4519600ee1374eb131a159c384422ea/.github/workflows/ci.yaml#L65-L74

On that note, maybe the `"nightly"` in this PR should be `"nightly-2022-11-01"`?

> In any case I think a config format change and a version change are two separate changes and should certainly be done in separate commits. And a version change should have some convincing reasoning (ideally also explained in the commit message).

Sure, I can separate them, but the end goal here was to make my IDEA IDE (and probably other things?) understand it should use the nightly toolchain, since 

https://github.com/charliermarsh/ruff/blob/81db00a3c4519600ee1374eb131a159c384422ea/README.md#L1654

---

_Comment by @not-my-profile on 2023-01-20 10:10_

Right I do not see any reason why we the CI should run `cargo test` only on nightly ... apparently that was accidentally changed in 79ca66ace5ebba10e94cda9925d19fdfc50ac327 ... I just opened #2019 to change it back.

And I also don't think changing the whole developer toolchain to nightly just because of `cargo fmt`/clippy makes much sense ... unfortunately there does not appear to be a cross-editor way of configuring just `cargo fmt` to use nightly ... for rust-analyzer with VSCode you can apparently use [this config](https://github.com/rust-lang/vscode-rust/issues/438#issuecomment-1003671382).

---

_Renamed from "Convert rust-toolchain file to rust-toolchain.toml" to "Convert rust-toolchain file to rust-toolchain.toml (with nightly)" by @akx on 2023-01-20 10:22_

---

_Comment by @akx on 2023-01-20 10:26_

Okay, I guess this can be closed with #2019 – but I hope you can see why I thought to change to `nightly` here, given the instructions and CI setup? 😄 

---

_Comment by @akx on 2023-01-20 10:36_

@not-my-profile btw, I get this trying to `cargo test` without nightly:

```
error[E0554]: `#![feature]` may not be used on the stable release channel
  --> /Users/akx/.cargo/registry/src/github.com-1ecc6299db9ec823/rustix-0.36.5/src/lib.rs:99:26
   |
99 | #![cfg_attr(rustc_attrs, feature(rustc_attrs))]
   |                          ^^^^^^^^^^^^^^^^^^^^
```

Not sure what in particular I'm doing wrong there...

---

_Comment by @not-my-profile on 2023-01-20 10:38_

Yes I now understand your reasoning :) The current `rust-toolchain` is indeed in conflict with the current instructions in the README.

> Not sure what in particular I'm doing wrong there...

Have you tried a `cargo clean`?

---

_Comment by @akx on 2023-01-20 10:50_

Closing in favor of #2019.

---

_Closed by @akx on 2023-01-20 10:50_

---
