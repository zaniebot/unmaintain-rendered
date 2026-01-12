```yaml
number: 1083
title: "Cannot compile 2.27.0 with `wrap_help` feature - unresolved import `term_size`"
type: issue
state: closed
author: mistodon
labels: []
assignees: []
created_at: 2017-10-25T21:06:11Z
updated_at: 2018-08-02T03:30:13Z
url: https://github.com/clap-rs/clap/issues/1083
synced_at: 2026-01-12T16:14:10Z
```

# Cannot compile 2.27.0 with `wrap_help` feature - unresolved import `term_size`

---

_@mistodon_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

Starting a blank project with clap 2.27.0 as a dependency, and enabling the `wrap_help` feature fails to compile.

### Rust Version

* rustc 1.21.0 (3b72af97e 2017-10-09)

### Affected Version of clap

* clap 2.27.0

### Expected Behavior Summary

Compiles successfully.

### Actual Behavior Summary

Fails to compile.

### Steps to Reproduce the issue

1.  Create a new binary project: `cargo init --bin`
2.  Add the following lines to Cargo.toml:
```toml
[dependencies.clap]
version = "2.27"
features = ["wrap_help"]
```
3.  Run `cargo build`
4. Observe the following error:
```
   Compiling clap v2.27.0
error[E0432]: unresolved import `term_size`
  --> /Users/username/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.27.0/src/app/help.rs:22:5
   |
22 | use term_size;
   |     ^^^^^^^^^ no `term_size` in the root

error: aborting due to previous error

error: Could not compile `clap`.
```

---

_Referenced in [clap-rs/clap#1084](../../clap-rs/clap/pulls/1084.md) on 2017-10-26 13:28_

---

_Comment by @kbknapp on 2017-10-26 13:29_

Thanks for filing! This is fixed in #1084 which might just be a temp fix until I find a better solution.

---

_Label `P1: urgent` added by @kbknapp on 2017-10-26 13:29_

---

_Label `Regression` added by @kbknapp on 2017-10-26 13:29_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-26 13:29_

---

_Closed by @kbknapp on 2017-10-26 15:05_

---
