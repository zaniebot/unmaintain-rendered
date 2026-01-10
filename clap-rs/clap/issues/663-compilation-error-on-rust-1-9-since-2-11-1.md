---
number: 663
title: Compilation error on Rust 1.9 since 2.11.1
type: issue
state: closed
author: i80and
labels: []
assignees: []
created_at: 2016-09-14T18:38:38Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/663
synced_at: 2026-01-10T01:26:33Z
---

# Compilation error on Rust 1.9 since 2.11.1

---

_Issue opened by @i80and on 2016-09-14 18:38_

When compiling Clap 2.11.1+ with Rust 1.9.0, I get the following error:

```
[...]/clap-2.12.1/src/errors.rs:857:35: 857:46 error: no method named `description` found for type `std::fmt::Error` in the current scope
[...]/clap-2.12.1/src/errors.rs:857         Error::with_description(e.description(), ErrorKind::Format)
```

If I had to guess, this is because of 58512f2fcb430745f1ee6ee8f1c67f62dc216c73.


---

_Comment by @kbknapp on 2016-09-14 23:03_

This could be because of colliding `clap::Error` and `std::fmt::Error`. Could you point to the code causing this error and I can see if both those structs are in scope?


---

_Label `T: RFC / question` added by @kbknapp on 2016-09-14 23:03_

---

_Label `C: errors` added by @kbknapp on 2016-09-14 23:03_

---

_Comment by @i80and on 2016-09-15 00:40_

I created a clean project to test, and can reproduce with the following:

```
extern crate clap;
fn main() {}
```


---

_Comment by @kbknapp on 2016-09-15 18:21_

Ah ok my mistake, I missed the Rust 1.9 part of the title. Since current `stable` is 1.11 I can't mark this as a bug. I believe even the rust-lang-nursery crates only have a requirement to compile two stable releases behind as well. 

However having said that, if someone knows how to detect `rustc` version and conditionally compile particular functions based on those findings I'm not against gating that method on anything prior to Rust 1.9.

@i80and is there a reason you can't update to the latest stable?


---

_Comment by @i80and on 2016-09-16 17:30_

My project is OpenBSD-focused, and since rustup doesn't support OpenBSD, it would be obnoxious to use a non-packaged version.

However, I understand that this isn't really your problem, and it's probably not worth adding kludges for this since 2.11.0 will work fine for the remainder of the Rust 1.9.0 lifecycle.

Thanks anyway!


---

_Closed by @i80and on 2016-09-16 17:30_

---

_Comment by @semarie on 2016-09-16 17:49_

@i80and if compiling a OpenBSD port isn't a problem for you, you could checkout the -current version of `lang/rust` and rebuild it for OpenBSD 6.0. The version is 1.11.0 and I expect it to be buildable without problem under 6.0.

```
$ cd /usr/ports/lang/rust && cvs up -A
$ pkg_add -a llvm cmake g++%4.9
$ make
$ doas make install
```


---

_Comment by @ryansname on 2016-09-27 09:23_

Just for documentations sake because I just ran into this with rustc 1.10.0. The problem is because `std::fmt::Error` on began impl'ing `std::err::Error` in rustc 1.11.0. 
See [`std::fmt::Error`](https://doc.rust-lang.org/1.11.0/std/fmt/struct.Error.html#method.description) and https://github.com/kbknapp/clap-rs/commit/58512f2fcb430745f1ee6ee8f1c67f62dc216c73#diff-1a3079550f2751fb09d74ca85b9d5dcbR829

Naturally updating to the latest stable will fix the problem for me also, I just hadn't gotten around to it yet.


---

_Referenced in [rust-lang/cargo#3131](../../rust-lang/cargo/issues/3131.md) on 2016-09-29 22:14_

---

_Referenced in [clap-rs/clap#713](../../clap-rs/clap/issues/713.md) on 2016-10-28 03:05_

---
