---
number: 4571
title: "Can't build with any version of Clap 4 due to rustix dependency"
type: issue
state: closed
author: ghuh
labels:
  - C-bug
assignees: []
created_at: 2022-12-22T18:13:15Z
updated_at: 2022-12-22T19:07:13Z
url: https://github.com/clap-rs/clap/issues/4571
synced_at: 2026-01-07T13:12:20-06:00
---

# Can't build with any version of Clap 4 due to rustix dependency

---

_Issue opened by @ghuh on 2022-12-22 18:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0 (69f9c33d7 2022-12-12)

### Clap Version

4.0.x (All patch versions up to at least 30)

### Minimal reproducible code

Just build with any version of Clap 4.0 and above.

### Steps to reproduce the bug with the above code

Need to be on Rust stable.

### Actual Behaviour

```
   Compiling rustix v0.36.5
error[E0554]: `#![feature]` may not be used on the stable release channel
  --> /Users/sambas/.cargo/registry/src/github.com-1ecc6299db9ec823/rustix-0.36.5/src/lib.rs:99:26
   |
99 | #![cfg_attr(rustc_attrs, feature(rustc_attrs))]
   |                          ^^^^^^^^^^^^^^^^^^^^

For more information about this error, try `rustc --explain E0554`.
error: could not compile `rustix` due to previous error
```

### Expected Behaviour

The build should complete.  If I downgrade to Clap 3.2 and change nothing else, it builds fine.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ghuh on 2022-12-22 18:13_

---

_Comment by @epage on 2022-12-22 18:52_

I believe we got our dependence on rustic in 4.0.27.

Unfortunately, I can't reproduce this
- I'm on 1.66 and updated my rustix version and I can't reproduce this.
- CI is running on all major platforms with rustix 0.35.13 and 0.36.4 in the dependency tree and building successfully.

---

_Comment by @ghuh on 2022-12-22 19:02_

I just tried reproducing on two other computers and the build worked fine on both of them so this must be something specific to the environment on this one computer.  Thanks for checking!

---

_Closed by @ghuh on 2022-12-22 19:02_

---

_Comment by @ghuh on 2022-12-22 19:07_

For posterity: this can be fixed with a simple `cargo clean`

---
