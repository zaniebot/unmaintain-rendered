---
number: 1414
title: "clap doesn't compile on macOS"
type: issue
state: closed
author: gedigi
labels: []
assignees: []
created_at: 2019-02-16T23:04:36Z
updated_at: 2019-06-21T13:21:32Z
url: https://github.com/clap-rs/clap/issues/1414
synced_at: 2026-01-10T01:26:52Z
---

# clap doesn't compile on macOS

---

_Issue opened by @gedigi on 2019-02-16 23:04_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.21.0 (3b72af97e 2017-10-09)

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

clap doesn't compile on macOS with rust 1.21.0, which according to the CHANGELOG is the lowest supported version. The problem appears to be in libc, which is a dependency of atty.

### Expected Behavior Summary

`cargo build` completes without errors.

### Actual Behavior Summary

```bash
$ cargo build
   Compiling libc v0.2.48
error: const fns are an unstable feature
    --> ~/.cargo/registry/src/github.com-1ecc6299db9ec823/libc-0.2.48/src/unix/bsd/apple/mod.rs:2388:42
     |
2388 |     const __DARWIN_ALIGNBYTES32: usize = mem::size_of::<u32>() - 1;
     |                                          ^^^^^^^^^^^^^^^^^^^^^
     |
     = help: in Nightly builds, add `#![feature(const_fn)]` to the crate attributes to enable

error: aborting due to previous error

error: Could not compile `libc`.
```


---

_Comment by @Dylan-DPC-zz on 2019-06-21 13:14_

thanks for reporting this. however since we are close to a 3.0 release, closing this as it isn't relevant anymore. 

---

_Closed by @Dylan-DPC-zz on 2019-06-21 13:14_

---

_Comment by @BurntSushi on 2019-06-21 13:21_

@gedigi Note that the [MSRV is Rust 1.31](https://github.com/clap-rs/clap/blob/784524f7eb193e35f81082cc69454c8c21b948f7/.travis.yml#L8), not Rust 1.21. Rust 1.21 is pretty ancient at this point.

---
