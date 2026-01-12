```yaml
number: 1321
title: memory bugs?
type: issue
state: closed
author: matthiaskrgr
labels:
  - E-help-wanted
assignees: []
created_at: 2018-07-18T13:54:20Z
updated_at: 2020-10-25T20:00:59Z
url: https://github.com/clap-rs/clap/issues/1321
synced_at: 2026-01-12T16:14:10Z
```

# memory bugs?

---

_@matthiaskrgr_

I was initially trying to build alacritty (branch: scollback /  commit b05ad74 ) https://github.com/jwilm/alacritty/ with memory sanitizer enabled
````
RUSTFLAGS="-Z sanitizer=memory" cargo run --target x86_64-unknown-linux-gnu
````
alacritty crashed and it looks like the crash happens inside clap (this is with clap = { version = "2", features = ["debug"] } )

````
    Finished dev [unoptimized + debuginfo] target(s) in 2m 18s
     Running `target/x86_64-unknown-linux-gnu/debug/alacritty`
==16011==WARNING: MemorySanitizer: use-of-uninitialized-value
    #0 0x55b4bd9fc284 in _$LT$alloc..vec..Vec$LT$T$GT$$u20$as$u20$core..ops..deref..Deref$GT$::deref::hc17fd4b273d5d3e1 /checkout/src/liballoc/vec.rs:1740:12
    #1 0x55b4bd64567d in _$LT$alloc..string..String$u20$as$u20$core..ops..deref..Deref$GT$::deref::hc00a8e740d725e26 /checkout/src/liballoc/string.rs:2043:42
    #2 0x55b4bd644959 in _$LT$alloc..string..String$u20$as$u20$core..fmt..Display$GT$::fmt::h0db76b99d125ea99 /checkout/src/liballoc/string.rs:1858:27
    #3 0x55b4bda2e4b1 in core::fmt::Formatter::run::hbd79213dcf5c8cbf /checkout/src/libcore/fmt/mod.rs:1103:8
    #4 0x55b4bda2e4b1 in core::fmt::write::h51a93c7f5efd3011 /checkout/src/libcore/fmt/mod.rs:1050
    #5 0x55b4bda110ca in std::io::Write::write_fmt::h24e3149f0acd1b0e /checkout/src/libstd/io/mod.rs:1182:14
    #6 0x55b4bda110ca in _$LT$std..io..stdio..Stdout$u20$as$u20$std..io..Write$GT$::write_fmt::he7138bd4cff19dfe /checkout/src/libstd/io/stdio.rs:459
    #7 0x55b4bda13a45 in std::io::stdio::print_to::_$u7b$$u7b$closure$u7d$$u7d$::h31d90f68beee29b3 /checkout/src/libstd/io/stdio.rs:686:8
    #8 0x55b4bda13a45 in _$LT$std..thread..local..LocalKey$LT$T$GT$$GT$::try_with::hee5d131df6506ede /checkout/src/libstd/thread/local.rs:294
    #9 0x55b4bda11824 in std::io::stdio::print_to::h3a29719b7d8231ef /checkout/src/libstd/io/stdio.rs:680:17
    #10 0x55b4bda11824 in std::io::stdio::_print::h38bd6db9fd791082 /checkout/src/libstd/io/stdio.rs:701
    #11 0x55b4bd4ab15a in clap::app::parser::Parser::propagate_settings::hc2651c29253b583b /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/parser.rs:385:8
    #12 0x55b4bd89c032 in clap::app::App::get_matches_from_safe_borrow::h1177565f3e4b47d4 /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1603:12
    #13 0x55b4bd89ad25 in clap::app::App::get_matches_from::h1641de1dada920d4 /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1513:8
    #14 0x55b4bd89aac6 in clap::app::App::get_matches::h1e8c0b268643791e /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.32.0/src/app/mod.rs:1455:49
    #15 0x55b4bbbf38fc in alacritty::cli::Options::load::h9375e2b14e1bb867 /home/matthias/vcs/github/alacritty/src/cli.rs:57:22
    #16 0x55b4bb38a1b8 in alacritty::main::hab4e721719a60555 /home/matthias/vcs/github/alacritty/src/main.rs:51:18
    #17 0x55b4bb386ad5 in std::rt::lang_start::_$u7b$$u7b$closure$u7d$$u7d$::h3e596b0415883f1a /checkout/src/libstd/rt.rs:74:33
    #18 0x55b4bda1f592 in std::rt::lang_start_internal::_$u7b$$u7b$closure$u7d$$u7d$::h16de3bbc1b71cc8e /checkout/src/libstd/rt.rs:59:12
    #19 0x55b4bda1f592 in _ZN3std9panicking3try7do_call17h34868cb42ce29260E.llvm.5762669851421814535 /checkout/src/libstd/panicking.rs:310
    #20 0x55b4bda2bb39 in __rust_maybe_catch_panic /checkout/src/libpanic_unwind/lib.rs:106:7
    #21 0x55b4bda0e7d5 in std::panicking::try::h2e0f9200bebc6716 /checkout/src/libstd/panicking.rs:289:12
    #22 0x55b4bda0e7d5 in std::panic::catch_unwind::h0064acbc425caa4f /checkout/src/libstd/panic.rs:392
    #23 0x55b4bda0e7d5 in std::rt::lang_start_internal::hbd6594748484e902 /checkout/src/libstd/rt.rs:58
    #24 0x55b4bb3869fe in std::rt::lang_start::h2cc1ea953e8a0adc /checkout/src/libstd/rt.rs:74:4
    #25 0x55b4bb38f131 in main (/home/matthias/vcs/github/alacritty/target/x86_64-unknown-linux-gnu/debug/alacritty+0x526131)
    #26 0x7f0a31e0b06a in __libc_start_main (/usr/lib/libc.so.6+0x2306a)
    #27 0x55b4bb270029 in _start (/home/matthias/vcs/github/alacritty/target/x86_64-unknown-linux-gnu/debug/alacritty+0x407029)
SUMMARY: MemorySanitizer: use-of-uninitialized-value /checkout/src/liballoc/vec.rs:1740:12 in _$LT$alloc..vec..Vec$LT$T$GT$$u20$as$u20$core..ops..deref..Deref$GT$::deref::hc17fd4b273d5d3e1
Exiting
````


So I went to the clap git repo ( master / 570b4e1a77f033dd99de587b6883afb0bb104399 ) and continued poking around with the sanitizers.
````cargo test```` passes
````cargo test --release```` fails #1320
````RUSTFLAGS="-Z sanitizer=memory" cargo test --target x86_64-unknown-linux-gnu```` segfaults:
````
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running target/x86_64-unknown-linux-gnu/debug/deps/clap-c8fb3e31fbf02681
error: process didn't exit successfully: `/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/clap-c8fb3e31fbf02681` (signal: 11, SIGSEGV: invalid memory reference)
````

I tried poking around with rust-lldb but I'm not sure if this helps:
````
Process 13031 launched: '/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/clap-c8fb3e31fbf02681' (x86_64)
Process 13031 stopped
* thread #1, name = 'clap-c8fb3e31fb', stop reason = signal SIGSEGV: invalid address (fault address: 0x2fffffffdc30)
    frame #0: 0x00005555556069c7 clap-c8fb3e31fbf02681`std::rt::lang_start::h1f7daf9f50143af5(main=&0x55555560d470, argc=1, argv=&0x7fffffffdd28) at rt.rs:74
(lldb) bt
* thread #1, name = 'clap-c8fb3e31fb', stop reason = signal SIGSEGV: invalid address (fault address: 0x2fffffffdc30)
  * frame #0: 0x00005555556069c7 clap-c8fb3e31fbf02681`std::rt::lang_start::h1f7daf9f50143af5(main=&0x55555560d470, argc=1, argv=&0x7fffffffdd28) at rt.rs:74
    frame #1: 0x000055555560da5d clap-c8fb3e31fbf02681`main + 77
    frame #2: 0x00007ffff71fd06b libc.so.6`__libc_start_main + 235
    frame #3: 0x00005555555db02a clap-c8fb3e31fbf02681`_start + 42
````

Next I tried the address sanitizer
````RUSTFLAGS="-Z sanitizer=address" cargo test --target x86_64-unknown-linux-gnu````
and got test failures (229) all across the board due to some linking failures :/
````
[...]
---- src/errors.rs - errors::ErrorKind::VersionDisplayed (line 345) stdout ----
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-L" "/home/matthias/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/tmp/rustdoctestQtVbQe/rust_out.rust_out0.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out1.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out10.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out11.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out12.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out13.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out14.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out15.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out2.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out3.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out4.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out5.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out6.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out7.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out8.rcgu.o" "/tmp/rustdoctestQtVbQe/rust_out.rust_out9.rcgu.o" "-o" "/tmp/rustdoctestQtVbQe/rust_out" "/tmp/rustdoctestQtVbQe/rust_out.crate.allocator.rcgu.o" "-Wl,--gc-sections" "-pie" "-Wl,-z,relro,-z,now" "-nodefaultlibs" "-L" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps" "-L" "/home/matthias/vcs/github/clap-rs/target/debug/deps" "-L" "/home/matthias/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libvec_map-b3d79fa534f4a3d9.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libtextwrap-9014ce980cef2303.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libunicode_width-8955b7ba7f12be16.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libstrsim-06344ee47596cbf9.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libbitflags-0cb13f9b16a8cd05.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libatty-870d48f2c75d049e.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/liblibc-26d8c1b0702fbed3.rlib" "/home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libansi_term-8937820d65c050ee.rlib" "-Wl,--start-group" "-L" "/home/matthias/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bdynamic" "-l" "std-4ff002878234ba38" "-Wl,--end-group" "-Wl,-Bstatic" "/home/matthias/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-a319cd5407aa2c9f.rlib" "-Wl,-Bdynamic" "-l" "util" "-l" "util" "-l" "dl" "-l" "rt" "-l" "pthread" "-l" "pthread" "-l" "gcc_s" "-l" "c" "-l" "m" "-l" "rt" "-l" "pthread" "-l" "util" "-l" "util"
  = note: /usr/local/bin/ld: error: undefined symbol: __asan_option_detect_stack_use_after_return
          >>> referenced by path.rs:1701 (/checkout/src/libstd/path.rs:1701)
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(std::path::Path::new::he256a8e799205e20) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_stack_malloc_0
          >>> referenced by path.rs:1701 (/checkout/src/libstd/path.rs:1701)
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(std::path::Path::new::he256a8e799205e20) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_report_store8
          >>> referenced by 15kq92zzbmxot4k9-f990bf537b0d29376dc22a3a104d97dc.rs
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(std::path::Path::new::he256a8e799205e20) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_report_store8
          >>> referenced by 15kq92zzbmxot4k9-f990bf537b0d29376dc22a3a104d97dc.rs
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(std::path::Path::new::he256a8e799205e20) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_report_load8
          >>> referenced by path.rs:1702 (/checkout/src/libstd/path.rs:1702)
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(std::path::Path::new::he256a8e799205e20) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_report_load8
          >>> referenced by path.rs:1702 (/checkout/src/libstd/path.rs:1702)
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(std::path::Path::new::he256a8e799205e20) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_init
          >>> referenced by 15kq92zzbmxot4k9-f990bf537b0d29376dc22a3a104d97dc.rs
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(asan.module_ctor) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_version_mismatch_check_v8
          >>> referenced by 15kq92zzbmxot4k9-f990bf537b0d29376dc22a3a104d97dc.rs
          >>>               clap-59e98bd757b6c1a5.15kq92zzbmxot4k9.rcgu.o:(asan.module_ctor) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_option_detect_stack_use_after_return
          >>> referenced by result.rs:745 (/checkout/src/libcore/result.rs:745)
          >>>               clap-59e98bd757b6c1a5.16u6js6g0l3k1ic6.rcgu.o:(_$LT$core..result..Result$LT$T$C$$u20$E$GT$$GT$::unwrap_or_else::h10f03766c0eb19b1) in archive /home/matthias/vcs/github/clap-rs/target/x86_64-unknown-linux-gnu/debug/deps/libclap-59e98bd757b6c1a5.rlib
          /usr/local/bin/ld: error: undefined symbol: __asan_stack_malloc_0
          >>> referenced by result.rs:745 (/checkout/src/libcore/result.rs:745)
````

Any ideas what is happening here?

### Rust Version

rustc 1.29.0-nightly (4f3c7a472 2018-07-17)




---

_Comment by @BurntSushi on 2018-07-18 14:00_

Please try switching to the system allocator to rule out the possibility that jemalloc (the default Rust allocation at time of writing) is causing false positives. e.g., https://github.com/rust-lang/regex/issues/460#issuecomment-376620297

---

_Comment by @matthiaskrgr on 2018-07-18 14:34_

Thanks Andrew!

I tried this now:
````diff
diff --git a/src/lib.rs b/src/lib.rs
index a5be32e6..7cc0c7d2 100644
--- a/src/lib.rs
+++ b/src/lib.rs
@@ -523,7 +523,6 @@
 // Lints we'd like to deny but are currently failing for upstream crates
 //      unused_qualifications       (bitflags, clippy)
 //      trivial_numeric_casts       (bitflags)
-#![cfg_attr(not(any(feature = "lints", feature = "nightly")), forbid(unstable_features))]
 #![cfg_attr(feature = "lints", feature(plugin))]
 #![cfg_attr(feature = "lints", plugin(clippy))]
 // Need to disable deny(warnings) while deprecations are active
@@ -532,6 +531,8 @@
 #![cfg_attr(feature = "lints", allow(doc_markdown))]
 #![cfg_attr(feature = "lints", allow(explicit_iter_loop))]
+#![feature(global_allocator, allocator_api, alloc_system)]
+extern crate alloc_system;
 #[cfg(all(feature = "color", not(target_os = "windows")))]
 extern crate ansi_term;
 #[cfg(feature = "color")]
````
but nothing changed;
memory sanitizer still crashes with "invalid memory reference"
Address sanitizer still got linking errors.

Am I doing this wrong?

---

_Comment by @BurntSushi on 2018-07-18 14:37_

@matthiaskrgr That doesn't look like a sufficient diff to me. Please see https://github.com/rust-lang/regex/issues/460#issuecomment-376620297 again. e.g., I don't actually see any use of `#[global_allocator]`. I also don't know what the semantics of setting a global allocator in a library are. Please create a `main` program that sets it.

---

_Comment by @kbknapp on 2018-07-21 01:49_

Interesting, thanks for the details and I'm curious what's causing this (and if it's jemalloc like @BurntSushi  suggested).

I've done a little digging so far, and I'm not entirely sure what's going on. A simple program such as this will trigger the memory sanitizer

```rust
#![feature(global_allocator, allocator_api)]

extern crate clap;
use clap::App;

// Uncommenting the following triggers and error ("conflicting #[global_allocator] in rustc_msan")
// use std::heap::System;
// #[global_allocator]
// static A: System = System;

fn main() {
    App::new("foo");
}
```
The error I'm getting is:

```
Running `target/x86_64-unknown-linux-gnu/debug/fake`
==10579==WARNING: MemorySanitizer: use-of-uninitialized-value
    #0 0x5557d9f6c9a0  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0x8f9a0)
    #1 0x5557d9f6caff  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0x8faff)
    #2 0x5557d9f6c899  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0x8f899)
    #3 0x5557d9ee837f  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xb37f)
    #4 0x5557d9ee8061  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xb061)
    #5 0x5557d9ee85ef  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xb5ef)
    #6 0x5557d9ee8d2c  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xbd2c)
    #7 0x5557d9ee8c07  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xbc07)
    #8 0x5557d9f6e877  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0x91877)
    #9 0x5557d9f803ae  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xa33ae)
    #10 0x5557d9f70c95  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0x93c95)
    #11 0x5557d9ee8b44  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xbb44)
    #12 0x5557d9ee8da1  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xbda1)
    #13 0x7f1efd679b96  (/lib/x86_64-linux-gnu/libc.so.6+0x21b96)
    #14 0x5557d9ee7949  (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0xa949)

SUMMARY: MemorySanitizer: use-of-uninitialized-value (/home/kevin/Projects/fake/target/x86_64-unknown-linux-gnu/debug/fake+0x8f9a0) 
Exiting
```

This is strange because clap insn't doing anything at this point other than initializing structs. This even happens on the v3-master branch where it's doing even less initialization.

I can't say I know enough to dig much deeper though :worried: 

---

_Label `help wanted` added by @kbknapp on 2018-07-21 01:50_

---

_Comment by @CreepySkeleton on 2020-07-02 02:46_

@pksunkara IF you have a linux machine, could you please run the example above and see if the warnings still trigger?

---

_Comment by @pksunkara on 2020-07-10 12:52_

I don't, I use Mac. But the VM provided by Kevin should be linux IIRC.

---

_Comment by @CreepySkeleton on 2020-09-18 18:14_

OK, finally found some time to get back to it.

The patient:

```rust
// We don't need to account for jemalloc these days

use clap::App;

fn main() {
    App::new("app");
}
```

I set WSL (Windows subsystem for linux) and went strait for the big gun - valgrind.

```
==802== Memcheck, a memory error detector
==802== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==802== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
==802== Command: ./target/debug/probe
==802==
==802== error calling PR_SET_PTRACER, vgdb might block
==802==
==802== HEAP SUMMARY:
==802==     in use at exit: 0 bytes in 0 blocks
==802==   total heap usage: 12 allocs, 12 frees, 5,015 bytes allocated
==802==
==802== All heap blocks were freed -- no leaks are possible
==802==
==802== For counts of detected and suppressed errors, rerun with: -v
==802== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

Clean and tidy.

MemorySanitizer, what do you have to say?

```
error: failed to run custom build command for `bitflags v1.2.1`

Caused by:
  process didn't exit successfully: `/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build` (exit code: 77)
  --- stderr
  Uninitialized bytes in __interceptor_memchr at offset 0 inside [0x701000000000, 4)
  ==902==WARNING: MemorySanitizer: use-of-uninitialized-value
      #0 0x7fca5d49ddee  (/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build+0x9ddee)
      #1 0x7fca5d4a62d0  (/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build+0xa62d0)
      #2 0x7fca5d475e7b  (/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build+0x75e7b)
      #3 0x7fca5d47b48e  (/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build+0x7b48e)
      #4 0x7fca5c5d1b96  (/lib/x86_64-linux-gnu/libc.so.6+0x21b96)
      #5 0x7fca5d40ca39  (/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build+0xca39)

  SUMMARY: MemorySanitizer: use-of-uninitialized-value (/mnt/d/workspace/probe/target/debug/build/bitflags-4a5ac15c6e640b31/build-script-build+0x9ddee)
  Exiting
warning: build failed, waiting for other jobs to finish...
error: build failed
```

So the `bitflags`' build script is misbehaving, but how? [They don't use unsafe or anything outside of std](https://github.com/bitflags/bitflags/blob/1.2.1/build.rs). Fine, let's fork it and remove the script:

```
error: failed to run custom build command for `hashbrown v0.8.2`

Caused by:
  process didn't exit successfully: `/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build` (exit code: 77)
  --- stderr
  Uninitialized bytes in __interceptor_memchr at offset 0 inside [0x701000000000, 4)
  ==1178==WARNING: MemorySanitizer: use-of-uninitialized-value
      #0 0x7fabc2ce66fe  (/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build+0xe66fe)
      #1 0x7fabc2cef2f0  (/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build+0xef2f0)
      #2 0x7fabc2c6b22b  (/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build+0x6b22b)
      #3 0x7fabc2c6bfbe  (/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build+0x6bfbe)
      #4 0x7fabc1dd1b96  (/lib/x86_64-linux-gnu/libc.so.6+0x21b96)
      #5 0x7fabc2c0e329  (/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build+0xe329)

  SUMMARY: MemorySanitizer: use-of-uninitialized-value (/mnt/d/workspace/probe/target/debug/build/hashbrown-7707856e3fe17cb4/build-script-build+0xe66fe)
  Exiting
warning: build failed, waiting for other jobs to finish...
error: build failed
```

So it fails with every build script, doesn't it. Screw it, I'm going with the hypotheses of "MemorySanitizer is broken" and report it to... [Oh](https://github.com/rust-lang/rust/issues/54365#issuecomment-683334262)

> As mentioned earlier, MemorySanitizer requires all code to be instrumented. The documentation describes how to instrument the Rust standard library using cargo build-std functionality.
> ...
> MemorySanitizer requires all program code to be instrumented. C/C++ dependencies need to be recompiled using Clang with -fsanitize=memory option. Failing to achieve that will result in false positive reports.

(Remember kids: always search for already existing issues and scrim through the old comments!)

I'm not going to do all that. It will be boring and very likely useless. Let's go with `sanitizer=address` instead.

```
$ RUSTFLAGS="-Z sanitizer=address" cargo +nightly run
   Compiling autocfg v1.0.1
   Compiling unicode-width v0.1.8
   Compiling vec_map v0.8.2
   Compiling bitflags v1.2.1 (/mnt/d/workspace/bitflags)
   Compiling os_str_bytes v2.3.2
   Compiling textwrap v0.12.1
   Compiling hashbrown v0.8.2
   Compiling indexmap v1.5.1
   Compiling clap v3.0.0-beta.1 (/mnt/d/workspace/clap)
   Compiling probe v0.2.0 (/mnt/d/workspace/probe)
    Finished dev [unoptimized + debuginfo] target(s) in 2m 02s
     Running `target/debug/probe`
```

So far, so good.

Well, it was obvious that the patient couldn't have a serious pathology, but what about something more complex? 

I checked all examples we have with valgrind. Full log is [here](https://gist.github.com/CreepySkeleton/a79d5fdfdf492f1e1efd32b1ccd9f709). We see there are a number of leaks in there. But is that because they are actual leaks or because clap exits via the `exit` syscall?

Oops, my time is up. I'll check next time.



---

_Comment by @lnicola on 2020-10-25 19:38_

I see no actual leaks in that Valgrind log.

---

_Comment by @pksunkara on 2020-10-25 19:46_

@lnicola I do see some like [these](https://gist.github.com/CreepySkeleton/a79d5fdfdf492f1e1efd32b1ccd9f709#file-gistfile1-txt-L20-L26). Am I mistaken and am missing some knowledge?

---

_Comment by @lnicola on 2020-10-25 19:58_

"Still reachable" means that the memory is still reachable (allocated) at the end of the program. It could be a leak, but it's normal if you call `exit` and the advise is to clean up and exit early instead of trying to free every bit of memory you've allocated (which might even hit the swap).

---

_Comment by @pksunkara on 2020-10-25 20:00_

Ah, okay. Thanks for looking into it.

I am closing this issue. If anyone else has the same problem, please re-open a new one.

---

_Closed by @pksunkara on 2020-10-25 20:00_

---
