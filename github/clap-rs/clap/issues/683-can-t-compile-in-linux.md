---
number: 683
title: "Can't compile in linux"
type: issue
state: closed
author: laishulu
labels: []
assignees: []
created_at: 2016-10-06T10:37:30Z
updated_at: 2018-08-02T03:29:54Z
url: https://github.com/clap-rs/clap/issues/683
synced_at: 2026-01-07T13:12:19-06:00
---

# Can't compile in linux

---

_Issue opened by @laishulu on 2016-10-06 10:37_

output of `uname -a`

```
Linux vultr.guest 3.13.0-92-generic #139-Ubuntu SMP Tue Jun 28 20:42:26 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

`Cargo install` gives:

```
Already up-to-date.
    Updating registry `https://github.com/rust-lang/crates.io-index`
 Downloading clap v2.14.0
   Compiling clap v2.14.0
/root/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.14.0/src/errors.rs:857:35: 857:46 error: no method named `description` found for type `std::fmt::Error` in the current scope
/root/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.14.0/src/errors.rs:857         Error::with_description(e.description(), ErrorKind::Format)
```


---

_Comment by @nabijaczleweli on 2016-10-06 16:28_

More relevant is your `rustc --version` and `cargo --version`


---

_Comment by @laishulu on 2016-10-06 17:13_

```
root@vultr:~# rustc --version
rustc 1.10.0 (cfcb716cf 2016-07-03)
root@vultr:~# cargo --version
cargo 0.11.0-nightly (259324c 2016-05-20)
```


---

_Comment by @nabijaczleweli on 2016-10-06 17:14_

Update Rust, try again


---

_Comment by @laishulu on 2016-10-06 17:30_

OK now


---

_Closed by @laishulu on 2016-10-06 17:30_

---

_Referenced in [git-series/git-series#29](../../git-series/git-series/issues/29.md) on 2016-10-19 05:16_

---
