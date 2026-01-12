```yaml
number: 2395
title: "ignore `0.4.19` is not compatible with rust 1.62.0 and under"
type: issue
state: closed
author: iitalics
labels:
  - wontfix
assignees: []
created_at: 2023-01-11T18:28:51Z
updated_at: 2023-08-30T15:08:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2395
synced_at: 2026-01-12T16:13:24Z
```

# ignore `0.4.19` is not compatible with rust 1.62.0 and under

---

_@iitalics_

```
$ rustup default
1.62.0-x86_64-unknown-linux-gnu (default)

$ cat Cargo.toml 
[package]
name = "scratch"
version = "0.1.0"
edition = "2018"

[dependencies]
ignore = "0.4.19"

$ cat src/main.rs
fn main() {}

$ cargo build
   Compiling memchr v2.5.0
   Compiling log v0.4.17
   Compiling regex-syntax v0.6.28
   Compiling cfg-if v1.0.0
   Compiling fnv v1.0.7
   Compiling same-file v1.0.6
   Compiling once_cell v1.17.0
   Compiling lazy_static v1.4.0
   Compiling walkdir v2.3.2
   Compiling thread_local v1.1.4
   Compiling aho-corasick v0.7.20
   Compiling bstr v1.1.0
   Compiling regex v1.7.1
   Compiling globset v0.4.10
   Compiling ignore v0.4.19
error[E0658]: use of unstable library feature 'scoped_threads'
    --> /home/milo/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.19/src/walk.rs:1285:9
     |
1285 |         std::thread::scope(|s| {
     |         ^^^^^^^^^^^^^^^^^^
     |
     = note: see issue #93203 <https://github.com/rust-lang/rust/issues/93203> for more information

error[E0658]: use of unstable library feature 'scoped_threads'
    --> /home/milo/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.19/src/walk.rs:1299:32
     |
1299 |                 handles.push(s.spawn(|| worker.run()));
     |                                ^^^^^
     |
     = note: see issue #93203 <https://github.com/rust-lang/rust/issues/93203> for more information

error[E0658]: use of unstable library feature 'scoped_threads'
    --> /home/milo/.cargo/registry/src/github.com-1ecc6299db9ec823/ignore-0.4.19/src/walk.rs:1302:24
     |
1302 |                 handle.join().unwrap();
     |                        ^^^^
     |
     = note: see issue #93203 <https://github.com/rust-lang/rust/issues/93203> for more information

For more information about this error, try `rustc --explain E0658`.
error: could not compile `ignore` due to 3 previous errors
warning: build failed, waiting for other jobs to finish...
```

it looks like it is using features that were nightly only in previous versions of rust. perhaps the crate should use the `rust-version =` field to indicate which versions it is compatible with. i've check that it works with rust 1.64.0

---

_Comment by @BurntSushi on 2023-01-11 18:31_

See #2391.

---

_Label `wontfix` added by @BurntSushi on 2023-08-30 15:08_

---

_Closed by @BurntSushi on 2023-08-30 15:08_

---
