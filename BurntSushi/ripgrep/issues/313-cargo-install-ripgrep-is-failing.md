```yaml
number: 313
title: cargo install ripgrep is failing
type: issue
state: closed
author: justbur
labels:
  - bug
assignees: []
created_at: 2017-01-10T04:47:05Z
updated_at: 2017-01-11T20:28:24Z
url: https://github.com/BurntSushi/ripgrep/issues/313
synced_at: 2026-01-12T16:13:21Z
```

# cargo install ripgrep is failing

---

_@justbur_

Here's the output. I'm not familiar with rust, but I'm happy to provide additional information.

```
$ cargo install ripgrep
    Updating registry `https://github.com/rust-lang/crates.io-index`
   Compiling winapi-build v0.1.1
   Compiling unicode-segmentation v0.1.3
   Compiling regex-syntax v0.3.9
   Compiling winapi v0.2.8
   Compiling fnv v1.0.5
   Compiling bytecount v0.1.5
   Compiling utf8-ranges v0.1.3
.cargo/registry/src/github.com-1ecc6299db9ec823/bytecount-0.1.5/src/lib.rs:151:45: 151:48 error: use of unstable library feature 'iter_arith': bounds recently changed (see issue #27739)
.cargo/registry/src/github.com-1ecc6299db9ec823/bytecount-0.1.5/src/lib.rs:151         self[..].iter().map(ByteChunk::sum).sum()
                                                                                                                           ^~~
error: aborting due to previous error
Build failed, waiting for other jobs to finish...
```

---

_Comment by @BurntSushi on 2017-01-10 11:50_

It looks like one of ripgrep's dependencies increased the minimum Rust version and didn't do a semver version bump.

This is still strange though. [ripgrep 0.3.2 has `bytecount 0.1.4` in its `Cargo.lock`](https://github.com/BurntSushi/ripgrep/blob/0.3.2/Cargo.lock#L42-L43), which I think should cause `cargo install ripgrep` to use `bytecount 0.1.4`. Instead, it seems to be picking up `bytecount 0.1.5` but I'm not sure why.

cc @llogiq, @alexcrichton

---

_Label `bug` added by @BurntSushi on 2017-01-10 11:51_

---

_Comment by @BurntSushi on 2017-01-10 11:52_

@justbur You have a few work-arounds available to you:

1. Upgrade your version of Rust. I'm assuming you're using a version older than 1.11.0 (you can check by running `rustc --version`) and try `cargo install ripgrep` again.
2. [Download a binary release.](https://github.com/BurntSushi/ripgrep/releases)

---

_Comment by @llogiq on 2017-01-10 13:26_

Sorry, my fault. Will fix shortly.

---

_Comment by @justbur on 2017-01-10 14:33_

@BurntSushi makes sense to me. 

I'm on Ubuntu 16.10 currently, using rust from the repos. My rustc is version 1.10.0, so yes you're right. Just tested the binary and it works fine. I'm not that interested in maintaining a different version of rust at the moment. 

I was just reporting because I felt like I had a pretty common setup and the instructions on the README failed for me. 

Thanks for the help


---

_Comment by @llogiq on 2017-01-10 16:22_

@justbur, again, my apologies. I'll try to fix as soon as possible (but had too little time for my last attempt). It's literally a one-line fix.

---

_Comment by @BurntSushi on 2017-01-10 16:28_

FWIW, I think this is strange behavior from Cargo. I don't think it should be using `bytecount 0.1.5` when the lockfile says `0.1.4`.

---

_Comment by @alexcrichton on 2017-01-10 17:00_

@BurntSushi ah yes I think this is a bug in Cargo where `cargo install` doesn't respect lock files: https://github.com/rust-lang/cargo/issues/2263

---

_Closed by @BurntSushi on 2017-01-10 23:30_

---

_Comment by @justbur on 2017-01-11 20:21_

ok, I just tried again, and I got past the initial error but ran into another one. Here's the output

```
$ cargo install ripgrep 
    Updating registry `https://github.com/rust-lang/crates.io-index`
   Compiling unicode-segmentation v0.1.3
   Compiling winapi-build v0.1.1
   Compiling regex-syntax v0.3.9
   Compiling fnv v1.0.5
   Compiling ansi_term v0.9.0
   Compiling libc v0.2.19
   Compiling unicode-width v0.1.4
   Compiling void v1.0.2
   Compiling utf8-ranges v0.1.3
   Compiling unreachable v0.1.1
   Compiling crossbeam v0.2.10
   Compiling bytecount v0.1.6
   Compiling same-file v0.1.1
   Compiling thread-id v3.0.0
   Compiling num_cpus v1.2.1
   Compiling thread_local v0.3.2
   Compiling memchr v0.1.11
   Compiling walkdir v1.0.7
   Compiling memmap v0.5.0
   Compiling aho-corasick v0.5.3
   Compiling kernel32-sys v0.2.2
   Compiling bitflags v0.7.0
   Compiling termcolor v0.1.1
   Compiling winapi v0.2.8
   Compiling lazy_static v0.2.2
   Compiling strsim v0.5.2
   Compiling thread-id v2.0.0
   Compiling ctrlc v2.0.1
   Compiling term_size v0.2.1
   Compiling log v0.3.6
   Compiling vec_map v0.6.0
   Compiling thread_local v0.2.7
   Compiling clap v2.19.3
.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.19.3/src/errors.rs:882:35: 882:46 error: no method named `description` found for type `std::fmt::Error` in the current scope
.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.19.3/src/errors.rs:882         Error::with_description(e.description(), ErrorKind::Format)
                                                                                                                ^~~~~~~~~~~
error: aborting due to previous error
Build failed, waiting for other jobs to finish...
error: failed to compile `ripgrep v0.3.2`, intermediate artifacts can be found at `/tmp/cargo-install.noglV3JyQm5G`

Caused by:
  Could not compile `clap`.
```

---

_Comment by @justbur on 2017-01-11 20:22_

Actually that looks like it might be due to having an older version of the std library. 

---

_Comment by @BurntSushi on 2017-01-11 20:23_

ripgrep `0.3` requires Rust 1.11 or newer unfortunately. You can try installing an older version of ripgrep:

    cargo install --vers 0.2.9 ripgrep

---

_Comment by @justbur on 2017-01-11 20:26_

@BurntSushi That explains it. Thanks. Maybe you could add a note to that effect next to the `cargo install ripgrep` line in the readme? 

---

_Comment by @BurntSushi on 2017-01-11 20:28_

@justbur Yeah. I think the minimum version is elsewhere in the README but it would definitely make sense to also put it with the `cargo install` step! :-)

---
