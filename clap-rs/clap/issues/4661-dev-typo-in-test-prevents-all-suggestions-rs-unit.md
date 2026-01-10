---
number: 4661
title: "[DEV] Typo in test prevents all `suggestions.rs` unit tests from running (incidentally all failing)"
type: issue
state: closed
author: corneliusroemer
labels:
  - C-bug
assignees: []
created_at: 2023-01-23T15:53:38Z
updated_at: 2023-01-23T16:17:13Z
url: https://github.com/clap-rs/clap/issues/4661
synced_at: 2026-01-10T01:27:59Z
---

# [DEV] Typo in test prevents all `suggestions.rs` unit tests from running (incidentally all failing)

---

_Issue opened by @corneliusroemer on 2023-01-23 15:53_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0 (69f9c33d7 2022-12-12)

### Clap Version

master

### Minimal reproducible code

None necessary

### Steps to reproduce the bug with the above code

Change a unit test in `suggestion.rs` so that it should fail and observe that it doesn't fail because it's not run (rust-analyzer also tells you that in VScode)

See https://github.com/clap-rs/clap/blob/9f4f341604266b82d50b6b71999962585c11b40f/src/parser/features/suggestions.rs#L75

### Actual Behaviour

Tests should run

### Expected Behaviour

Tests don't run

### Additional Context

Reason: it seems that this is due to a typo:

This:
```rs
#[cfg(all(test, feature = "suggestions"))]
```
https://github.com/clap-rs/clap/blob/9f4f341604266b82d50b6b71999962585c11b40f/src/parser/features/suggestions.rs#L75

should read
```rs
#[cfg(all(test, feature = "suggestion"))]
```

Instead, so `feature` instead of `features`. See: https://doc.rust-lang.org/cargo/reference/features.html for reference


### Debug Output

_No response_

---

_Label `C-bug` added by @corneliusroemer on 2023-01-23 15:53_

---

_Referenced in [clap-rs/clap#4662](../../clap-rs/clap/pulls/4662.md) on 2023-01-23 15:58_

---

_Referenced in [clap-rs/clap#4664](../../clap-rs/clap/pulls/4664.md) on 2023-01-23 16:06_

---

_Closed by @epage on 2023-01-23 16:17_

---
