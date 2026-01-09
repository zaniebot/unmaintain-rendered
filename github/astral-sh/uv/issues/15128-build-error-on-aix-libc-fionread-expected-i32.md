---
number: 15128
title: "Build error on AIX: libc::FIONREAD: expected i32, found i64"
type: issue
state: closed
author: mrichter-vw
labels:
  - bug
  - external
assignees: []
created_at: 2025-08-07T11:50:21Z
updated_at: 2025-08-19T10:27:44Z
url: https://github.com/astral-sh/uv/issues/15128
synced_at: 2026-01-07T13:12:19-06:00
---

# Build error on AIX: libc::FIONREAD: expected i32, found i64

---

_Issue opened by @mrichter-vw on 2025-08-07 11:50_

### Summary

Mainly a bug in dependencies but to track the fix in uv itself. The libc bug will be fixed in next version. See Links down below.

```
# also happens by appending--release
$ cargo build --offline
[...]
   Compiling jobserver v0.1.33
error[E0308]: mismatched types
    --> /data/rust/uv-0.8.5/vendor/jobserver/src/unix.rs:356:57
     |
356  |         cvt(unsafe { libc::ioctl(self.read.as_raw_fd(), libc::FIONREAD, len.as_mut_ptr()) })?;
     |                      -----------                        ^^^^^^^^^^^^^^ expected `i32`, found `i64`
     |                      |
     |                      arguments to this function are incorrect
     |
note: function defined here
    --> /data/rust/uv-0.8.5/vendor/libc/src/unix/aix/mod.rs:2994:12
     |
2994 |     pub fn ioctl(fildes: c_int, request: c_int, ...) -> c_int;
     |            ^^^^^
help: you can convert an `i64` to an `i32` and panic if the converted value doesn't fit
     |
356  |         cvt(unsafe { libc::ioctl(self.read.as_raw_fd(), libc::FIONREAD.try_into().unwrap(), len.as_mut_ptr()) })?;
     |                                                                       ++++++++++++++++++++

   Compiling hashbrown v0.15.4
For more information about this error, try `rustc --explain E0308`.
error: could not compile `jobserver` (lib) due to 1 previous error
warning: build failed, waiting for other jobs to finish...
```

- Issue in jobserver-rs: https://github.com/rust-lang/jobserver-rs/issues/112
- PR in libc: https://github.com/rust-lang/libc/pull/4582

### Platform

AIX

### Version

0.8.5

### Python version

_No response_

---

_Label `bug` added by @mrichter-vw on 2025-08-07 11:50_

---

_Label `external` added by @zanieb on 2025-08-07 13:05_

---

_Comment by @zanieb on 2025-08-07 13:05_

Thanks!

---

_Referenced in [astral-sh/uv#15376](../../astral-sh/uv/pulls/15376.md) on 2025-08-19 10:15_

---

_Closed by @konstin on 2025-08-19 10:27_

---
