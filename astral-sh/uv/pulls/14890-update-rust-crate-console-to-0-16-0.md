```yaml
number: 14890
title: Update Rust crate console to 0.16.0
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: console-0.16
created_at: 2025-07-25T11:16:31Z
updated_at: 2025-07-25T16:09:42Z
url: https://github.com/astral-sh/uv/pull/14890
synced_at: 2026-01-12T16:11:28Z
```

# Update Rust crate console to 0.16.0

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This revisits https://github.com/astral-sh/uv/pull/14364, which was opened by the renovate bot and originally failed with an error I don’t quite understand in https://github.com/astral-sh/uv/pull/14364#issuecomment-3017545431.

Since 852aba4f90988b7bd437573b721062228d504b49 updated to `indicatif` 0.18, we now already have `console` 0.16 in the dependency tree. This PR adjusts the direct dependency on `console` to match.

The only breaking change in [`console` 0.16.0](https://github.com/console-rs/console/releases/tag/0.16.0) is that crates that depend on `console` with `default-features = False` may need to explicitly enable the new `std` feature. This is the case for `uv`: while I did find that `cargo test` passes with just the `console` dependency version adjusted, this is due to [feature unification](https://doc.rust-lang.org/cargo/reference/features.html#feature-unification), i.e., the indirect dependency on `console` via `indicatif` 0.18 already requires its `std` feature. We can see by inspection that `uv` should also have a direct dependency on `console` with the `std` feature. For example, see:

https://github.com/astral-sh/uv/blob/05031becc3762b8b298e98046d6514a6cb6fb244/crates/uv-console/src/lib.rs#L1

and note that `Term` is gated by the `std` feature in

https://github.com/console-rs/console/blob/a51fcead7cda1fc6f5ac552a5588aaba8c069639/src/lib.rs#L90-L93

The addition of `features = ["std"]` is the key difference between this PR and https://github.com/astral-sh/uv/pull/14364.

## Test Plan

<!-- How was it tested? -->
`cargo test`


---

_@zanieb reviewed on 2025-07-25 13:00_

---

_Review comment by @zanieb on `Cargo.lock`:762 on 2025-07-25 13:00_

Hm why did this change?

---

_Comment by @zanieb on 2025-07-25 13:00_

Thank you!

---

_@musicinmybrain reviewed on 2025-07-25 13:06_

---

_Review comment by @musicinmybrain on `Cargo.lock`:762 on 2025-07-25 13:06_

I have no idea! I simply ran `cargo update console@0.15.11 console@0.16.0` using the system Rust toolchain on an up-to-date Fedora 42 installation. I’m happy to investigate, if you have any ideas.

You could also check out this branch, `git reset --hard HEAD~1` to drop the commit that updated `Cargo.lock`, and try doing the `Cargo.lock` update yourself – maybe you will end up with a different result?

---

_Merged by @zanieb on 2025-07-25 16:09_

---

_Closed by @zanieb on 2025-07-25 16:09_

---

_Label `internal` added by @zanieb on 2025-07-25 16:09_

---
