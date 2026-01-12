```yaml
number: 1132
title: Compilation failure under rust nightly webassembly target
type: issue
state: closed
author: smarter
labels:
  - C-enhancement
  - E-medium
  - E-easy
assignees: []
created_at: 2017-12-20T01:06:40Z
updated_at: 2018-08-02T03:30:16Z
url: https://github.com/clap-rs/clap/issues/1132
synced_at: 2026-01-12T16:14:10Z
```

# Compilation failure under rust nightly webassembly target

---

_@smarter_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.24.0-nightly (dc39c3169 2017-12-17)

### Affected Version of clap

* 2.29.0

Rust now has a [webassembly target](https://www.hellorust.com/setup/wasm-target/), trying to use it in a project that depends on `clap` results in a build failure:
```rust
     Running `rustc --crate-name clap /home/smarter/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.29.0/src/lib.rs --crate-type lib --emit=dep-info,link -C debuginfo=2 --cfg 'feature="ansi_term"' --cfg 'feature="atty"' --cfg 'feature="color"' --cfg 'feature="default"' --cfg 'feature="strsim"' --cfg 'feature="suggestions"' --cfg 'feature="vec_map"' -C metadata=65f4e6c3f5bc989d -C extra-filename=-65f4e6c3f5bc989d --out-dir /home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps --target wasm32-unknown-unknown -L dependency=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps -L dependency=/home/smarter/opt/rav1e/target/debug/deps --extern unicode_width=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libunicode_width-d8e4ab5b3b6e196a.rlib --extern bitflags=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libbitflags-6b084702002cf111.rlib --extern ansi_term=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libansi_term-ac6664a6dc50e79a.rlib --extern vec_map=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libvec_map-c395abf84ff3b365.rlib --extern textwrap=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libtextwrap-216ed0ff2756cb7b.rlib --extern atty=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libatty-ee3084e679e50df7.rlib --extern strsim=/home/smarter/opt/rav1e/target/wasm32-unknown-unknown/debug/deps/libstrsim-64624fb788d914f4.rlib --cap-lints allow`
error[E0433]: failed to resolve. Could not find `unix` in `os`
   --> /home/smarter/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.29.0/src/app/parser.rs:662:26
    |
662 |             use std::os::unix::ffi::OsStrExt;
    |                          ^^^^ Could not find `unix` in `os`

error[E0433]: failed to resolve. Could not find `unix` in `os`
 --> /home/smarter/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.29.0/src/args/arg.rs:8:14
  |
8 | use std::os::unix::ffi::OsStrExt;
  |              ^^^^ Could not find `unix` in `os`

...
```

It might be impossible to actually use clap for anything with the wasm backend right now, but it would be nice if it still managed to build.

---

_Comment by @kbknapp on 2018-01-09 17:11_

I agree. I haven't really put any thought into wasm because I can't imagine parsing arguments in the browser, but you're right it'd be nice if it still builds.

This would probably be an easy fix, and if anyone wants to add a non-default cargo feature `wasm` and do the fixes I'd be happy to mentor it.

---

_Label `T: enhancement` added by @kbknapp on 2018-01-09 17:12_

---

_Label `P4: nice to have` added by @kbknapp on 2018-01-09 17:12_

---

_Label `D: easy` added by @kbknapp on 2018-01-09 17:12_

---

_Label `M: mentored` added by @kbknapp on 2018-01-09 17:12_

---

_Label `good first issue` added by @kbknapp on 2018-01-09 17:12_

---

_Comment by @rtsao on 2018-01-10 02:50_

I think the clap use case for the wasm target would be consuming it from Node.js. I guess in theory you could build a CLI entirely in Rust then ship it to npm.

---

_Comment by @stevepentland on 2018-02-10 18:31_

@kbknapp I'm interested in taking this one on if no one else has started

---

_Comment by @kbknapp on 2018-02-11 18:56_

That would be great! Thanks @stevepentland

---

_Comment by @stevepentland on 2018-02-11 20:05_

@kbknapp I need to ask about what you meant with a non-default wasm cargo feature? I've been looking through the code at how to reconcile the changes and could use a little insight from you.

---

_Comment by @kbknapp on 2018-02-11 20:27_

By non-default I just meant a cargo feature that it's not included in the default features array (and thus opt-in instead of opt-out). However, I now think there are better ways to solve this.

A better solution would be to add some additional OsStrExt traits inside `src/osstringext.rs` following the OsStrExt3 example for Windows, since Windows is essentially the same problem as wasm.

Actually...now that I think about it, maybe just reusing the OsStrExt3 that Windows uses would work too? That just require changing the cfg statements...hmm I'd try that out first.


So in order of things I'd try:

* Reusing the OsStrExt3 for Windows and wasn't
* Adding a new trait just like OsStrExt3 but for was specifically

---

_Comment by @stevepentland on 2018-02-11 20:37_

Alright sounds good, I’ll start there and see how it goes. Thanks!

---

_Referenced in [clap-rs/clap#1187](../../clap-rs/clap/pulls/1187.md) on 2018-02-19 04:09_

---

_Referenced in [clap-rs/clap#1190](../../clap-rs/clap/pulls/1190.md) on 2018-02-23 20:49_

---

_Comment by @kbknapp on 2018-02-23 20:59_

Closed with #1190 thanks to @stevepentland :tada:

---

_Closed by @kbknapp on 2018-02-23 20:59_

---

_Comment by @jamesray1 on 2018-05-05 08:09_

Getting this error with https://github.com/Drops-of-Diamond/diamond_drops/issues/59 with Clap 2.31.2.

---

_Referenced in [clap-rs/clap#1270](../../clap-rs/clap/issues/1270.md) on 2018-05-05 22:18_

---
