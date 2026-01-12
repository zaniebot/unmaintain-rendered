```yaml
number: 2272
title: Ripgrep panics when used with rustc
type: issue
state: closed
author: ms747
labels:
  - invalid
assignees: []
created_at: 2022-08-01T07:35:21Z
updated_at: 2022-08-01T11:18:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2272
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep panics when used with rustc

---

_@ms747_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Windows 11

#### Describe your bug.

When you pipe anything from rustc to rg it panics.

#### What are the steps to reproduce the behavior?

rustc | rg 
panic

rustc --print target-list | rg aarch 
I/O error: operation failed to complete synchronously

#### What is the actual behavior?

```
$ rustc --print target-list | rg --debug aarch
DEBUG|grep_regex::literal|C:\Users\Mayur Shah\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-regex-0.1.9\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(aarch)], limit_size: 250, limit_class: 10 }
DEBUG|grep_cli::decompress|C:\Users\Mayur Shah\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-cli-0.1.6\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|C:\Users\Mayur Shah\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-cli-0.1.6\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|C:\Users\Mayur Shah\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-cli-0.1.6\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|globset|C:\Users\Mayur Shah\.cargo\registry\src\github.com-1ecc6299db9ec823\globset-0.4.8\src\lib.rs:421: built glob set; 0 literals, 0 basenames, 9 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
I/O error: operation failed to complete synchronously
```

#### What is the expected behavior?

```
rustc --print target-list | rg aarch64
aarch64-apple-darwin
aarch64-apple-ios
aarch64-apple-ios-macabi
aarch64-apple-ios-sim
aarch64-apple-tvos
aarch64-apple-watchos-sim
aarch64-fuchsia
aarch64-kmc-solid_asp3
aarch64-linux-android
aarch64-nintendo-switch-freestanding
aarch64-pc-windows-gnullvm
aarch64-pc-windows-msvc
aarch64-unknown-freebsd
aarch64-unknown-hermit
aarch64-unknown-linux-gnu
aarch64-unknown-linux-gnu_ilp32
aarch64-unknown-linux-musl
aarch64-unknown-netbsd
aarch64-unknown-none
aarch64-unknown-none-softfloat
aarch64-unknown-openbsd
aarch64-unknown-redox
aarch64-unknown-uefi
aarch64-uwp-windows-msvc
aarch64-wrs-vxworks
aarch64_be-unknown-linux-gnu
aarch64_be-unknown-linux-gnu_ilp32
```


---

_Comment by @BurntSushi on 2022-08-01 11:01_

I don't see any panic? All I see is an I/O error. And I don't know where that's coming from. I won't be testing on my Windows machine for quite some time, so you or someone else might want to do some additional debugging here.

---

_Label `question` added by @BurntSushi on 2022-08-01 11:02_

---

_Label `help wanted` added by @BurntSushi on 2022-08-01 11:02_

---

_Comment by @BurntSushi on 2022-08-01 11:03_

My guess is that there is something wrong with your environment. Probably some weird Windows thing to be honest.

---

_Comment by @ms747 on 2022-08-01 11:07_

Here's the panic message.

```
$ rustc | rg
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] -e PATTERN ... [PATH ...]
    rg [OPTIONS] -f PATTERNFILE ... [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN
    rg [OPTIONS] --help
    rg [OPTIONS] --version

For more information try --help

thread 'main' panicked at 'failed printing to stdout: The pipe is being closed. (os error 232)', library\std\src\io\stdio.rs:1016:9
stack backtrace:
   0:         0x66a0b6e0 - std::backtrace_rs::backtrace::dbghelp::trace::hcf1edc4340169add
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\..\..\backtrace\src\backtrace/dbghelp.rs:98:5
   1:         0x66a0b6e0 - std::backtrace_rs::backtrace::trace_unsynchronized::h667c1c2dcc656354
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\..\..\backtrace\src\backtrace/mod.rs:66:5
   2:         0x66a0b6e0 - std::sys_common::backtrace::_print_fmt::hfb7bace0584ade55
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\sys_common/backtrace.rs:66:5
   3:         0x66a0b6e0 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hb33b4e98d0091546
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\sys_common/backtrace.rs:45:22
   4:         0x66a70e4a - core::fmt::write::h6c05da133b140855
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\core\src\fmt/mod.rs:1196:17
   5:         0x669fd373 - std::io::Write::write_fmt::h9b6cb30f5a3cd549
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\io/mod.rs:1654:15
   6:         0x66a0eb09 - std::sys_common::backtrace::_print::hc9d14608fb2479ef
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\sys_common/backtrace.rs:48:5
   7:         0x66a0eb09 - std::sys_common::backtrace::print::hbd6a7f88953bf46c
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\sys_common/backtrace.rs:35:9
   8:         0x66a0eb09 - std::panicking::default_hook::{{closure}}::h9cbf1d70d95153f7
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:295:22
   9:         0x66a0e78d - std::panicking::default_hook::h6361d2e804ad32c1
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:314:9
  10:         0x673df389 - rustc_driver[4502434ebcdcaeba]::DEFAULT_HOOK::{closure#0}::{closure#0}
  11:         0x66a0f3db - std::panicking::rust_panic_with_hook::h9759101d5af84392
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:702:17
  12:         0x66a0f125 - std::panicking::begin_panic_handler::{{closure}}::h3400764062bd15ad
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:588:13
  13:         0x66a0c2e7 - std::sys_common::backtrace::__rust_end_short_backtrace::h199fe7c55d65b172
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\sys_common/backtrace.rs:138:18
  14:         0x66a0ee59 - rust_begin_unwind
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:584:5
  15:         0x66a6d6f5 - core::panicking::panic_fmt::haf27162bd71f9b2c
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\core\src/panicking.rs:142:14
  16:         0x669fac0b - std::io::stdio::print_to::h67a6ba4112d4d800
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\io/stdio.rs:1016:9
  17:         0x669fac0b - std::io::stdio::_print::h4f77d1fb452cb5a1
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src\io/stdio.rs:1028:5
  18:         0x673dc2f3 - rustc_driver[4502434ebcdcaeba]::usage
  19:         0x673de2b1 - rustc_driver[4502434ebcdcaeba]::handle_options
  20:         0x673d7d5b - <rustc_driver[4502434ebcdcaeba]::RunCompiler>::run
  21:         0x673715ca - <core[be4d9e6022649537]::panic::unwind_safe::AssertUnwindSafe<rustc_driver[4502434ebcdcaeba]::main::{closure#0}> as core[be4d9e6022649537]::ops::function::FnOnce<()>>::call_once
  22:         0x673e064d - rustc_driver[4502434ebcdcaeba]::main
  23:           0x40164e - rustc_main[80d93e91ab353e27]::main
  24:           0x4015b6 - std[c1538c24ed0124a6]::sys_common::backtrace::__rust_begin_short_backtrace::<fn(), ()>
  25:           0x40160c - std[c1538c24ed0124a6]::rt::lang_start::<()>::{closure#0}
  26:         0x669ed75a - core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once::h392d7cb81e6352b4
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3\library\core\src\ops/function.rs:280:13
  27:         0x669ed75a - std::panicking::try::do_call::h131f29fc06b4f4bb
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:492:40
  28:         0x669ed75a - std::panicking::try::h2b4cdc89938fcb7d
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:456:19
  29:         0x669ed75a - std::panic::catch_unwind::h327f69bb8fe3e3f9
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panic.rs:137:14
  30:         0x669ed75a - std::rt::lang_start_internal::{{closure}}::h050c0e344fae99eb
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/rt.rs:128:48
  31:         0x669ed75a - std::panicking::try::do_call::hf5df717773459fcf
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:492:40
  32:         0x669ed75a - std::panicking::try::h724c59bb637e9cac
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panicking.rs:456:19
  33:         0x669ed75a - std::panic::catch_unwind::h2c5424824164171e
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/panic.rs:137:14
  34:         0x669ed75a - std::rt::lang_start_internal::h71b2fce70fe28f35
                               at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library\std\src/rt.rs:128:20
  35:           0x401688 - main
  36:           0x4013f8 - __tmainCRTStartup
  37:           0x40151b - mainCRTStartup
  38:     0x7ffec8a354e0 - <unknown>
  39:     0x7ffec9c2485b - <unknown>

error: internal compiler error: unexpected panic

note: the compiler unexpectedly panicked. this is a bug.

note: we would appreciate a bug report: https://github.com/rust-lang/rust/issues/new?labels=C-bug%2C+I-ICE%2C+T-compiler&template=ice.md

note: rustc 1.62.1 (e092d0b6b 2022-07-16) running on x86_64-pc-windows-gnu

query stack during panic:
end of query stack
```

I think it's mostly a `rustc` issue more than ripgrep's.

---

_Closed by @ms747 on 2022-08-01 11:07_

---

_Comment by @BurntSushi on 2022-08-01 11:18_

Yes, especially given that the panic came from rustc and not ripgrep.

Definitely helpful to include full details in bug reports from the get-go in the future. Thanks.

---

_Label `help wanted` removed by @BurntSushi on 2022-08-01 11:18_

---

_Label `question` removed by @BurntSushi on 2022-08-01 11:18_

---

_Label `invalid` added by @BurntSushi on 2022-08-01 11:18_

---
