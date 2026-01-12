```yaml
number: 697
title: "MSAN: use of uninitialized memory"
type: issue
state: closed
author: matthiaskrgr
labels: []
assignees: []
created_at: 2017-11-29T08:30:21Z
updated_at: 2018-01-31T02:36:30Z
url: https://github.com/BurntSushi/ripgrep/issues/697
synced_at: 2026-01-12T16:13:22Z
```

# MSAN: use of uninitialized memory

---

_@matthiaskrgr_

compile with
````ASAN_OPTIONS=detect_odr_violation=0  RUSTFLAGS="-Z sanitizer=memory"    cargo test   --target x86_64-unknown-linux-gnu````
and let tests run:
````
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running target/x86_64-unknown-linux-gnu/debug/deps/rg-b724ffcf965ef356
Uninitialized bytes in __interceptor_memchr at offset 14 inside [0x702000000080, 24)
==23066==WARNING: MemorySanitizer: use-of-uninitialized-value
    #0 0x562af6e5e498 in std::sys::unix::memchr::memchr /checkout/src/libstd/sys/unix/memchr.rs:18
    #1 0x562af6e5e498 in std::memchr::memchr /checkout/src/libstd/memchr.rs:35
    #2 0x562af6e5e498 in std::ffi::c_str::{{impl}}::_new /checkout/src/libstd/ffi/c_str.rs:335
    #3 0x562af6e5e498 in std::ffi::c_str::{{impl}}::new<&[u8]> /checkout/src/libstd/ffi/c_str.rs:331
    #4 0x562af6e5e498 in _ZN3std3sys4unix2fs4cstr17hcad796a86497483cE.llvm.CC2D16CA /checkout/src/libstd/sys/unix/fs.rs:558
    #5 0x562af6e5e55c in std::sys::unix::fs::stat::h6455c0f362ed72c6 /checkout/src/libstd/sys/unix/fs.rs:733
    #6 0x562af6e4417a in std::fs::metadata<&std::path::PathBuf> /checkout/src/libstd/fs.rs:1289
    #7 0x562af6e4417a in term::terminfo::searcher::get_dbpath_for_term::hce5b5ed1e66507f1 /checkout/src/libterm/terminfo/searcher.rs:60
    #8 0x562af6e462e6 in term::terminfo::TermInfo::from_name::h4f3d71af0acf5032 /checkout/src/libterm/terminfo/mod.rs:99
    #9 0x562af6e460b7 in term::terminfo::TermInfo::from_env::h43802c2db156b935 /checkout/src/libterm/terminfo/mod.rs:85
    #10 0x562af6e46d34 in _$LT$term..terminfo..TerminfoTerminal$LT$T$GT$$GT$::new::h240111c97948f44e /checkout/src/libterm/terminfo/mod.rs:244
    #11 0x562af6e3d824 in term::stdout::h131658d513cb547b /checkout/src/libterm/lib.rs:78
    #12 0x562af6e1b01e in _ZN40_$LT$test..ConsoleTestState$LT$T$GT$$GT$3new17h34bc068b308f8107E.llvm.6287DFAC /checkout/src/libtest/lib.rs:567
    #13 0x562af6e1cc68 in test::run_tests_console::h78ac09c85a1945af /checkout/src/libtest/lib.rs:964
    #14 0x562af6e19a76 in test::test_main::hbb445b25f2975ee3 /checkout/src/libtest/lib.rs:291
    #15 0x562af6e19d44 in test::test_main_static::h4cccd3700c5284cd /checkout/src/libtest/lib.rs:327
    #16 0x562af66a6d01 in rg::__test::main::ha2ab9912f8381927 /home/matthias/vcs/github/ripgrep/src/main.rs:1
    #17 0x562af6e6b88e in __rust_maybe_catch_panic /checkout/src/libpanic_unwind/lib.rs:101
    #18 0x562af6e55a93 in std::panicking::try<(),closure> /checkout/src/libstd/panicking.rs:459
    #19 0x562af6e55a93 in std::panic::catch_unwind<closure,()> /checkout/src/libstd/panic.rs:365
    #20 0x562af6e55a93 in std::rt::lang_start::ha0e932603d3a14ad /checkout/src/libstd/rt.rs:58
    #21 0x562af66a6d71 in main (/home/matthias/vcs/github/ripgrep/target/x86_64-unknown-linux-gnu/debug/deps/rg-b724ffcf965ef356+0x1d7d71)
    #22 0x7f0b0b73f430 in __libc_start_main (/lib64/libc.so.6+0x20430)
    #23 0x562af64f8d19 in encoding_rs::data::map_with_ranges /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs-0.7.1/src/data.rs:19061
    #24 0x562af64f8d19 in encoding_rs::data::gb18030_range_decode /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs-0.7.1/src/data.rs:19088
    #25 0x562af64f8d19 in _start /home/matthias/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs-0.7.1/src/gb18030.rs:264

SUMMARY: MemorySanitizer: use-of-uninitialized-value /checkout/src/libstd/sys/unix/memchr.rs:18 in std::sys::unix::memchr::memchr
Exiting
error: test failed, to rerun pass '--bin rg'
````

---

_Renamed from "MSAN: use of uninizialized memory" to "MSAN: use of uninitialized memory" by @matthiaskrgr on 2017-11-29 08:36_

---

_Comment by @BurntSushi on 2017-11-29 10:22_

What is our confidence level that this isn't a false positive? I know valgrind has issues with false positives when we use jemalloc, which I believe is still enabled by default. In any case, from looking at the stack trace, it looks like this bug (if it is a true positive) is in rustc's testing infrastructure, so this would be the wrong place to file it. My next step would be to minimize the test case.

cc @alexcrichton

---

_Comment by @matthiaskrgr on 2017-11-29 11:01_

I've been finding a lot of bugs in c/c++ projects using the sanitizers supplied by gcc/clang and I don't remember encountering a false positive there, however I don't know how the implementation in rust is.
I stopped using valgrind long ago because of false positives and runtime performance. 

---

_Comment by @alexcrichton on 2017-11-29 14:50_

@BurntSushi I haven't used msan much myself but I've seen this exact same use of uninitialized memory and this trace before with valgrind on the same crate. May be a false positive? Not sure...

---

_Comment by @BurntSushi on 2017-11-29 14:52_

All right, I'll take a deeper dive on this when I get a chance and try to minimize it. Just wanted to check first to make sure it wasn't a known/understood issue before spending time on it!

---

_Comment by @matthiaskrgr on 2017-11-29 14:53_

For the record: I used rust nightly to repro this.

---

_Comment by @BurntSushi on 2017-11-29 14:57_

@matthiaskrgr In case it matters, can you show the output of `rustc --version` and `cargo --version`? e.g.,

```
$ rustc --version
rustc 1.22.0-nightly (0e6f4cf51 2017-09-27)
$ cargo --version
cargo 0.23.0-nightly (8118b02ac 2017-09-14)
```

---

_Comment by @matthiaskrgr on 2017-11-29 15:23_

````
rustc 1.24.0-nightly (73bca2b9f 2017-11-28)
cargo 0.24.0-nightly (abd137ad1 2017-11-12)
````
repo @ 7ae1f373c2b899c7db5f8106dec4d7423b1d8364

---

_Comment by @BurntSushi on 2017-11-29 15:29_

@matthiaskrgr Thanks!

---

_Comment by @BurntSushi on 2018-01-31 02:36_

So I tried to reproduce this and got:

```
[andrew@Cheetah ripgrep] ASAN_OPTIONS=detect_odr_violation=0 RUSTFLAGS="-Z sanitizer=memory" cargo test 
   Compiling same-file v1.0.1
   Compiling regex-syntax v0.4.2
   Compiling bitflags v1.0.1
   Compiling ansi_term v0.10.2
   Compiling utf8-ranges v1.0.0
   Compiling cfg-if v0.1.2
   Compiling strsim v0.6.0
   Compiling bytecount v0.3.1
   Compiling unicode-width v0.1.4
   Compiling termcolor v0.3.3 (file:///home/andrew/code/rust/ripgrep/termcolor)
   Compiling fnv v1.0.6
   Compiling lazy_static v1.0.0
   Compiling crossbeam v0.3.2
   Compiling libc v0.2.34
   Compiling vec_map v0.8.0
   Compiling void v1.0.2
   Compiling log v0.4.0
   Compiling encoding_rs v0.7.1
   Compiling unreachable v1.0.0
   Compiling textwrap v0.9.0
   Compiling thread_local v0.3.5
   Compiling walkdir v2.0.1
   Compiling log v0.3.9
   Compiling memchr v2.0.1
   Compiling memmap v0.6.2
   Compiling num_cpus v1.8.0
   Compiling atty v0.2.6
   Compiling clap v2.29.0
   Compiling aho-corasick v0.6.4
   Compiling env_logger v0.4.3
   Compiling regex v0.2.4
   Compiling globset v0.2.1 (file:///home/andrew/code/rust/ripgrep/globset)
   Compiling grep v0.1.7 (file:///home/andrew/code/rust/ripgrep/grep)
   Compiling ignore v0.3.1 (file:///home/andrew/code/rust/ripgrep/ignore)
   Compiling ripgrep v0.7.1 (file:///home/andrew/code/rust/ripgrep)
error: failed to run custom build command for `ripgrep v0.7.1 (file:///home/andrew/code/rust/ripgrep)`
process didn't exit successfully: `/home/andrew/code/rust/ripgrep/target/debug/build/ripgrep-b92b3a6c61f2f1b0/build-script-build` (exit code: 77)
--- stderr
==1319==WARNING: MemorySanitizer: use-of-uninitialized-value
    #0 0x55d908510678 in build_script_build::main::hf6b8c2b4c40416f2 /home/andrew/code/rust/ripgrep/build.rs:17
    #1 0x55d90852c9c7 in std::rt::lang_start::_$u7b$$u7b$closure$u7d$$u7d$::hb5d6752ed6556008 /checkout/src/libstd/rt.rs:74
    #2 0x55d9087b4687 in std::rt::lang_start_internal::_$u7b$$u7b$closure$u7d$$u7d$::h7f6a1730b8ce5654 /checkout/src/libstd/rt.rs:59
    #3 0x55d9087b4687 in _ZN3std9panicking3try7do_call17hb11c25abdfc27ad2E.llvm.17CB4BF5 /checkout/src/libstd/panicking.rs:479
    #4 0x55d9087c9b9e in __rust_maybe_catch_panic /checkout/src/libpanic_unwind/lib.rs:102
    #5 0x55d9087b8bf3 in std::panicking::try::h27415ddda2439ce6 /checkout/src/libstd/panicking.rs:458
    #6 0x55d9087b8bf3 in std::panic::catch_unwind::hbf670fa3783fabbc /checkout/src/libstd/panic.rs:358
    #7 0x55d9087b8bf3 in std::rt::lang_start_internal::ha082c8a960712b5c /checkout/src/libstd/rt.rs:58
    #8 0x55d90852c904 in std::rt::lang_start::h733eb01ed68bd1e1 /checkout/src/libstd/rt.rs:74
    #9 0x55d908510b11 in main (/home/andrew/code/rust/ripgrep/target/debug/build/ripgrep-b92b3a6c61f2f1b0/build-script-build+0x3db11)
    #10 0x7f6d3d6e6f49 in __libc_start_main (/usr/lib/libc.so.6+0x20f49)
    #11 0x55d9084e0b69 in _start (/home/andrew/code/rust/ripgrep/target/debug/build/ripgrep-b92b3a6c61f2f1b0/build-script-build+0xdb69)

SUMMARY: MemorySanitizer: use-of-uninitialized-value /home/andrew/code/rust/ripgrep/build.rs:17 in build_script_build::main::hf6b8c2b4c40416f2
Exiting
```

It's a `use-of-uninitialized-value` error, but it's from the build script. I'm not sure what's going on here, but it certainly feels like a false positive to me. I'm going to close this until we have stronger evidence. One thing that might be worth trying is using the system allocator instead of jemalloc.

---

_Closed by @BurntSushi on 2018-01-31 02:36_

---
