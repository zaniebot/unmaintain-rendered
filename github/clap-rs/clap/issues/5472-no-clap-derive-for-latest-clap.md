---
number: 5472
title: No clap_derive for latest clap
type: issue
state: closed
author: Cthutu
labels:
  - C-bug
assignees: []
created_at: 2024-04-23T12:55:12Z
updated_at: 2024-04-23T14:00:58Z
url: https://github.com/clap-rs/clap/issues/5472
synced_at: 2026-01-07T13:12:20-06:00
---

# No clap_derive for latest clap

---

_Issue opened by @Cthutu on 2024-04-23 12:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.77.2

### Clap Version

4.5.4

### Minimal reproducible code

```rust
fn main() {}
```


### Steps to reproduce the bug with the above code

Add clap using `cargo add clap -F derive`.

### Actual Behaviour

```
error: failed to select a version for the requirement `clap_derive = "=4.5.4"`
candidate versions found which didn't match: 4.4.7, 4.4.2, 4.4.0, ...
location searched: crates.io index
required by package `clap v4.5.4`
    ... which satisfies dependency `clap = "^4.5.4"` (locked to 4.5.4) of package `mchat v0.1.0 (C:\Users\Matt17\mud\mchat)`
if you are looking for the prerelease package it needs to be specified explicitly
    clap_derive = { version = "4.0.0-rc.1" }
perhaps a crate was updated and forgotten to be re-vendored?
```

### Expected Behaviour

Derive feature should be added without error.

### Additional Context

_No response_

### Debug Output

Can't compile clap.

---

_Label `C-bug` added by @Cthutu on 2024-04-23 12:55_

---

_Comment by @Cthutu on 2024-04-23 13:36_

By running `cargo update`, it seemed to fix itself.  Not sure if it is a local issue or not.

---

_Closed by @Cthutu on 2024-04-23 13:36_

---

_Comment by @epage on 2024-04-23 14:00_

Also tried those reproduction steps and they didn't work for me.  Sometimes cargo can get into weird states when multiple dependencies are present.  Were those your actual steps or was this only on an existing project?

---

_Referenced in [clap-rs/clap#5931](../../clap-rs/clap/issues/5931.md) on 2025-02-26 08:04_

---
