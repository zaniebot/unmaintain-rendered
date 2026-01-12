```yaml
number: 3865
title: Offline docs
type: issue
state: closed
author: pickfire
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2022-06-24T15:18:03Z
updated_at: 2022-06-30T16:32:57Z
url: https://github.com/clap-rs/clap/issues/3865
synced_at: 2026-01-12T16:14:15Z
```

# Offline docs

---

_@pickfire_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.6

### Describe your use case

`cargo doc --open` shows clap documentation that is not helpful when user is offline and noticed the docs required internet.

> https://github.com/clap-rs/clap

### Describe the solution you'd like

IIRC clap used to include module level docs, even some little example to get user started is fine or a high level view, or even including README is fine.

### Alternatives, if applicable

Users have to be online to read clap docs, which may not be available all the time anywhere (like on the plane).

### Additional Context

_No response_

---

_Label `C-enhancement` added by @pickfire on 2022-06-24 15:18_

---

_Comment by @epage on 2022-06-26 03:16_

If you run `cargo doc --features unstable-doc --open` then you will see something closer to `docs.rs`

We have several desires at odds
- We want to test the examples
- We want to single-source the README and top-level lib documentation
- We want to use features not available with the default features

From that combination, we require a feature flag to get the top-level module documentation.  We could ;potentially have an alternate example but we'd need to make sure it gets tested and kept up-to-date when most people won't be seeing it to notice it going stale (not just in whether it compiles but does it make sense at runtime and is it following best practices)

---

_Label `S-waiting-on-decision` added by @epage on 2022-06-26 03:16_

---

_Label `A-help` added by @epage on 2022-06-26 03:16_

---

_Comment by @pickfire on 2022-06-26 14:12_

If README could include another file, then this issue can be solved, but single-source file is probably hard in this case. But how do clap even test the README? If in lib.rs I undestand, but testing README.md code?

---

_Comment by @epage on 2022-06-28 01:38_

`lib.rs` pulls in the README when certain feature flags are set.  We then run the doc tests in that scenario, testing the readme's code sample.

---

_Comment by @pickfire on 2022-06-28 16:34_

Ah, that explains why the docs only shows up with `derive` but not with default features https://github.com/clap-rs/clap/blob/b4a1362486a95309c44f4ba9ef24038f1281f384/src/lib.rs#L8

Maybe we can update the existing docs to mention why it doesn't show the example docs? (because derive is not enabled)
```
//! <https://github.com/clap-rs/clap>
```

Or maybe we can include docs when it is not testing even with default features? https://doc.rust-lang.org/reference/conditional-compilation.html#test

```rust
#![cfg_attr(any(feature = "derive", not(test)), doc = include_str!("../README.md"))]
```

---

_Referenced in [clap-rs/clap#3886](../../clap-rs/clap/pulls/3886.md) on 2022-06-30 01:59_

---

_Comment by @epage on 2022-06-30 01:59_

> Or maybe we can include docs when it is not testing even with default features?

I just tested that solution and it doesn't quite work.  I'm assuming when the rustdoc code is being extracted, it isn't setting `test` for us to condition on it.

Going with the explanation

---

_Closed by @epage on 2022-06-30 16:32_

---
