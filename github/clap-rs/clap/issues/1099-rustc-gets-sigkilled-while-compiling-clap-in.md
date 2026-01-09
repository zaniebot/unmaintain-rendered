---
number: 1099
title: "`rustc` gets sigkilled while compiling Clap in release mode"
type: issue
state: closed
author: omern1
labels: []
assignees: []
created_at: 2017-11-09T04:57:13Z
updated_at: 2018-08-02T03:30:14Z
url: https://github.com/clap-rs/clap/issues/1099
synced_at: 2026-01-07T13:12:19-06:00
---

# `rustc` gets sigkilled while compiling Clap in release mode

---

_Issue opened by @omern1 on 2017-11-09 04:57_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version
`rustc 1.22.0-nightly (4750c1ec0 2017-10-19)`

### Affected Version of clap
`v2.27.1`

### Expected Behavior Summary
Clap should compile and run.

### Actual Behavior Summary
Clap does not compile, the compiler gets `SIGKILL`ed.

### Steps to Reproduce the issue
1. Include a crate that requires clap (in my case `comrak`).
2. Try to compile a release build.
3. See the magic happen.


### Sample Code or Link to Sample Code
```
   Compiling clap v2.26.2
     Running `rustc --crate-name clap /root/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.26.2/src/lib.rs --crate-type lib --emit=dep-info,link -C opt-level=3 --cfg 'feature="ansi_term"' --cfg 'feature="atty"' --cfg 'feature="color"' --cfg 'feature="default"' --cfg 'feature="strsim"' --cfg 'feature="suggestions"' --cfg'feature="term_size"' --cfg 'feature="wrap_help"' -C metadata=83ba9ae54b80579e -C extra-filename=-83ba9ae54b80579e --out-dir /var/www/nabeelomer.me/target/release/deps -L dependency=/var/www/nabeelomer.me/target/release/deps --extern atty=/var/www/nabeelomer.me/target/release/deps/libatty-8982c3c53cb6d381.rlib --extern unicode_width=/var/www/nabeelomer.me/target/release/deps/libunicode_width-2b00e4ad336ace05.rlib --extern strsim=/var/www/nabeelomer.me/target/release/deps/libstrsim-210abe1bc674c4e6.rlib --extern textwrap=/var/www/nabeelomer.me/target/release/deps/libtextwrap-0466aacf0efd6616.rlib --extern term_size=/var/www/nabeelomer.me/target/release/deps/libterm_size-ab087923b35103ed.rlib --extern vec_map=/var/www/nabeelomer.me/target/release/deps/libvec_map-3a36d765a0f37010.rlib --extern bitflags=/var/www/nabeelomer.me/target/release/deps/libbitflags-a597111cc5faeae3.rlib --extern ansi_term=/var/www/nabeelomer.me/target/release/deps/libansi_term-344a753cad51277a.rlib --cap-lints allow`
error: Could not compile `clap`.

Caused by:
  process didn't exit successfully: `rustc --crate-name clap /root/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.26.2/src/lib.rs --crate-type lib --emit=dep-info,link -C opt-level=3 --cfg feature="ansi_term" --cfg feature="atty" --cfg feature="color" --cfg feature="default" --cfg feature="strsim" --cfg feature="suggestions" --cfg feature="term_size" --cfg feature="wrap_help" -C metadata=83ba9ae54b80579e -C extra-filename=-83ba9ae54b80579e --out-dir /var/www/nabeelomer.me/target/release/deps -L dependency=/var/www/nabeelomer.me/target/release/deps --extern atty=/var/www/nabeelomer.me/target/release/deps/libatty-8982c3c53cb6d381.rlib --extern unicode_width=/var/www/nabeelomer.me/target/release/deps/libunicode_width-2b00e4ad336ace05.rlib --extern strsim=/var/www/nabeelomer.me/target/release/deps/libstrsim-210abe1bc674c4e6.rlib --extern textwrap=/var/www/nabeelomer.me/target/release/deps/libtextwrap-0466aacf0efd6616.rlib --extern term_size=/var/www/nabeelomer.me/target/release/deps/libterm_size-ab087923b35103ed.rlib --extern vec_map=/var/www/nabeelomer.me/target/release/deps/libvec_map-3a36d765a0f37010.rlib --extern bitflags=/var/www/nabeelomer.me/target/release/deps/libbitflags-a597111cc5faeae3.rlib --extern ansi_term=/var/www/nabeelomer.me/target/release/deps/libansi_term-344a753cad51277a.rlib --cap-lints allow` (signal: 9, SIGKILL: kill)
```

### Debug output

N/A


---

_Renamed from "`rustc` gets sigkilled while compiling " to "`rustc` gets sigkilled while compiling Clap in release mode" by @omern1 on 2017-11-09 04:58_

---

_Comment by @kbknapp on 2017-11-09 15:36_

I can't reproduce the issue. Could you compile single threaded, to ensure it's a clap issue? Also, what version of Rust are you using?

---

_Comment by @omern1 on 2017-11-09 19:26_

I've updated my `rustc` to `rustc 1.23.0-nightly (02004ef78 2017-11-08)`,  upgraded clap to `v2.26.2` and it still doesn't compile. I'm on Ubuntu Server 16.04 (Linux 4.4.0-1039-aws).

Also, I don't know how to compile it single threaded.

---

_Comment by @BurntSushi on 2017-11-09 19:28_

How much ram do you have on that machine? This smells like the Linux OOM killer.

---

_Comment by @omern1 on 2017-11-09 19:35_

512 MiB. Yeah... You are right... It is the OOM killer.

---

_Closed by @omern1 on 2017-11-09 19:35_

---

_Comment by @kbknapp on 2017-11-09 20:59_

I hadn't looked at memory usage of compiling Rust programs...especially on low memory machines (VPSs, etc.). Compiling a bare `cargo new fake --bin` takes ~116mb of RAM on my machine. Adding clap bumps it to ~485mb. 

---
