```yaml
number: 2019
title: Change CI to use MSRV for cargo test and build
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: stable-rust-in-ci
created_at: 2023-01-20T10:18:15Z
updated_at: 2023-01-20T12:41:57Z
url: https://github.com/astral-sh/ruff/pull/2019
synced_at: 2026-01-12T04:52:00Z
```

# Change CI to use MSRV for cargo test and build

---

_Pull request opened by @not-my-profile on 2023-01-20 10:18_

This was apparently accidentally changed in
79ca66ace5ebba10e94cda9925d19fdfc50ac327.

---

_@akx requested changes on 2023-01-20 10:24_

Could you also fix the readme accordingly?

It's saying

```shell
cargo +nightly test --all
```
right now – maybe "For development, we use" should be rephrased so it's clear that building and testing should happen with stable Rust, but `fmt` and `clippy` should be run with nightly?

---

_Comment by @not-my-profile on 2023-01-20 10:33_

I think our goal is to be as strict as possible. The `rustfmt` and `clippy` from nightly are stricter than their stable versions. However a successful `cargo test` on stable is stricter than a successful `cargo test` on nightly (since it cannot use unstable features) and more important (since we use stable for our release builds). So I think it makes sense that our CI enforces rustfmt & clippy on nightly and `cargo test` & `build` on stable.

> Could you also fix the readme accordingly?

Done :)

Couple of other thoughts:

* We may want to move that information to `CONTRIBUTING.md` ... I think it's a bit out of place in the README.
* We could link some editor-specific documentation in `CONTRIBUTING.md` on how to configure rustfmt/clippy to use nightly while using stable for the rest are alternatively tell the contributor to just run `rustup override set nightly` within the ruff repository (since a `cargo test` for ruff that passes on nightly is highly likely to also pass on stable since we don't use any unstable features).

---

_Renamed from "Change CI to use stable Rust for cargo test and build" to "Change CI to use MSRV for cargo test and build" by @not-my-profile on 2023-01-20 10:55_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:74 on 2023-01-20 12:38_

I assume we can just omit this line and `toolchain` now, since it matches the format file.

---

_@charliermarsh reviewed on 2023-01-20 12:38_

---

_Comment by @charliermarsh on 2023-01-20 12:39_

Read through the discussion here and in the previous issue. All makes sense. And yes, my intention was to use strictest settings for fmt and clippy (nightly is also required for rustfmt to support most configuration options), but to keep the code itself compiling under stable.

---

_Merged by @charliermarsh on 2023-01-20 12:39_

---

_Closed by @charliermarsh on 2023-01-20 12:39_

---

_@charliermarsh reviewed on 2023-01-20 12:40_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:74 on 2023-01-20 12:40_

(But, merging anyway, I don't mind if it's explicit. Welcome to change in a follow-up commit.)

---

_@not-my-profile reviewed on 2023-01-20 12:41_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:74 on 2023-01-20 12:41_

Ah yes according to the [action documentation](https://github.com/actions-rs/toolchain#the-toolchain-file) that should work.

---
