---
number: 2741
title: "error[E0658]: arbitrary expressions in key-value attributes are unstable"
type: issue
state: closed
author: happysalada
labels:
  - C-bug
assignees: []
created_at: 2021-08-28T12:36:35Z
updated_at: 2021-08-29T00:37:27Z
url: https://github.com/clap-rs/clap/issues/2741
synced_at: 2026-01-10T01:27:24Z
---

# error[E0658]: arbitrary expressions in key-value attributes are unstable

---

_Issue opened by @happysalada on 2021-08-28 12:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.54

### Clap Version

3.0.0-beta-4

### Minimal reproducible code

There is a compilation error on darwin and linux
the pueue crate which has a dependency on clap, will fail compiling from source with the following error

pueue> error[E0658]: arbitrary expressions in key-value attributes are unstable
pueue>  --> /private/tmp/nix-build-pueue-1.0.1.drv-0/pueue-1.0.1-vendor.tar.gz/clap/src/lib.rs:8:10
pueue>   |
pueue> 8 | #![doc = include_str!("../README.md")]
pueue>   |          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
pueue>   |
pueue>   = note: see issue #78835 <https://github.com/rust-lang/rust/issues/78835> for more information
pueue> error: aborting due to previous error
pueue> For more information about this error, try `rustc --explain E0658`.
pueue> error: could not compile `clap`
pueue> To learn more, run the command again with --verbose.
pueue> warning: build failed, waiting for other jobs to finish...
pueue> error: build failed


### Steps to reproduce the bug with the above code

Here is the link to the crate https://github.com/Nukesor/pueue
running cargo build from the src will trigger the error

### Actual Behaviour

compilation error

### Expected Behaviour

no compilation error

### Additional Context

This is for packaging for the NixOS distribution.

### Debug Output

_No response_

---

_Label `T: bug` added by @happysalada on 2021-08-28 12:36_

---

_Referenced in [Nukesor/pueue#235](../../Nukesor/pueue/issues/235.md) on 2021-08-28 12:37_

---

_Comment by @epage on 2021-08-28 15:40_

Thats strange.   In beta4, we bumped MSRV to 1.54 which [stablized this feature](https://github.com/rust-lang/rust/blob/master/RELEASES.md#version-1540-2021-07-29) but your report mentions using 1.54.  

---

_Comment by @happysalada on 2021-08-29 00:37_

My bad, I was actually on version 1.53, sorry for the bother!

---

_Closed by @happysalada on 2021-08-29 00:37_

---

_Referenced in [fornwall/rust-script#41](../../fornwall/rust-script/issues/41.md) on 2021-11-27 08:35_

---

_Referenced in [gatecat/prjoxide#32](../../gatecat/prjoxide/pulls/32.md) on 2022-09-24 21:15_

---
