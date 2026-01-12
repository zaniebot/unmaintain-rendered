```yaml
number: 22
title: Pipe causes panic
type: issue
state: closed
author: mlsteele
labels:
  - bug
assignees: []
created_at: 2016-09-23T17:59:45Z
updated_at: 2019-03-21T14:40:43Z
url: https://github.com/BurntSushi/ripgrep/issues/22
synced_at: 2026-01-12T16:13:21Z
```

# Pipe causes panic

---

_@mlsteele_

Running `rg --help | echo` causes a panic. Not that this is a particularly useful thing to do anyway. ;) This only happens for `--help`, works fine when searching.

```
$ rg --version
0.1.16
$ RUST_BACKTRACE=1 rg --help | echo

thread 'main' panicked at 'failed printing to stdout: Broken pipe (os error 32)', ../src/libstd/io/stdio.rs:617
stack backtrace:
   1:        0x1034311eb - std::sys::backtrace::tracing::imp::write::h46e546df6e4e4fe6
   2:        0x1034333ba - std::panicking::default_hook::_$u7b$$u7b$closure$u7d$$u7d$::h077deeda8b799591
   3:        0x103432fea - std::panicking::default_hook::heb8b6fd640571a4f
   4:        0x103425a68 - std::panicking::rust_panic_with_hook::hd7b83626099d3416
   5:        0x103433996 - std::panicking::begin_panic::h941ea76fc945d925
   6:        0x1034268d8 - std::panicking::begin_panic_fmt::h30280d4dd3f149f5
   7:        0x10342bdb2 - std::io::stdio::_print::h91aef6f665f00d62
   8:        0x103392ff2 - docopt::dopt::Error::exit::had75b1255cfb9a0a
   9:        0x10334003f - rg::main::h6a22bacbbfd7cdf6
  10:        0x103432bad - std::panicking::try::call::hca715a47aa047c49
  11:        0x1034339eb - __rust_maybe_catch_panic
  12:        0x1034329d1 - std::rt::lang_start::h162055cb2e4b9fe7
[1]    69118 abort      RUST_BACKTRACE=1 rg --help
```


---

_Comment by @houshuang on 2016-09-23 19:43_

Confirmed. Version 0.1.15, OSX (binary from site).


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:17_

---

_Comment by @BurntSushi on 2016-09-24 02:18_

Bah, yup, bug.


---

_Closed by @BurntSushi on 2016-09-24 23:12_

---

_Comment by @mlsteele on 2019-02-20 20:59_

Sorry it's back. I think the panic is [here](https://github.com/clap-rs/clap/blob/master/src/errors.rs#L401) now.

---

_Comment by @BurntSushi on 2019-02-20 21:05_

Please provide a reproduction.

On Wed, Feb 20, 2019, 15:59 Miles Steele <notifications@github.com wrote:

> Sorry it's back. I think the panic is here
> <https://github.com/clap-rs/clap/blob/master/src/errors.rs#L401> now.
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/22#issuecomment-465751769>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34uMztCrT7_1bT0H4PdPtZLGWW2t2ks5vPbc-gaJpZM4KFPHu>
> .
>


---

_Comment by @mlsteele on 2019-02-20 21:15_

```
$ git rev-parse HEAD
d9cf05ad50d976d55e2c5e1234fa3d03aad4bd66
$ export RUST_BACKTRACE=1
$ cargo run --help | grep
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
thread 'main' panicked at 'Error writing Error to stdout: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }', src/libcore/result.rs:997:5
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::_print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: rust_begin_unwind
   7: core::panicking::panic_fmt
   8: core::result::unwrap_failed
   9: clap::errors::Error::exit
  10: cargo::exit_with_error
  11: cargo::main
  12: std::rt::lang_start::{{closure}}
  13: std::panicking::try::do_call
  14: __rust_maybe_catch_panic
  15: std::rt::lang_start_internal
  16: main
$ sw_vers
ProductName:    Mac OS X
ProductVersion: 10.14.2
BuildVersion:   18C54
$ cargo --version
cargo 1.34.0-nightly (865cb7010 2019-02-10)
$ rustc --version
rustc 1.34.0-nightly (eac09088e 2019-02-15)
```

---

_Comment by @BurntSushi on 2019-02-20 21:21_

Why are you using `cargo run`? What happens when you use the binary directly? I would probably expect that bug to be in Cargo, not ripgrep. That --help flag is being given to Cargo. To give it to ripgrep, you would need `cargo run -- --help` I think.

(This is why providing a reproduction is always important.)

---

_Comment by @BurntSushi on 2019-02-20 21:23_

Yes, that backtrace points to Cargo, not ripgrep.

---

_Comment by @mlsteele on 2019-02-20 21:42_

Whoops. I was only using cargo (incorrectly) because I cloned the repo to poke around after seeing it another way.

### cargo install
This is how I've been using ripgrep normally. As you can see it's nondeterministic but mostly panics.
```
$ cargo install ripgrep --force
...
   Replacing /Users/miles/.cargo/bin/rg
$ ~/.cargo/bin/rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
$ ~/.cargo/bin/rg --help | grep
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
$ ~/.cargo/bin/rg --help | grep
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
thread 'main' panicked at 'Error writing Error to stdout: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }', src/libcore/result.rs:997:5
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::_print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: rust_begin_unwind
   7: core::panicking::panic_fmt
   8: core::result::unwrap_failed
   9: clap::errors::Error::exit
  10: clap::app::App::get_matches
  11: rg::args::Args::parse
  12: rg::main
  13: std::rt::lang_start::{{closure}}
  14: std::panicking::try::do_call
  15: __rust_maybe_catch_panic
  16: std::rt::lang_start_internal
  17: main
```

### github download
https://github.com/BurntSushi/ripgrep/releases
```
$ ~/Downloads/ripgrep-0.10.0-x86_64-apple-darwin/rg --version
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
$ ~/Downloads/ripgrep-0.10.0-x86_64-apple-darwin/rg --help | grep
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
thread 'main' panicked at 'Error writing Error to stdout: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }', libcore/result.rs:983:5
stack backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
```

### cargo run
fwiw `cargo run -- --help | grep` does not panic and `cargo run -- --help` outputs ripgrep's help not cargo's.
```
$ git rev-parse HEAD
d9cf05ad50d976d55e2c5e1234fa3d03aad4bd66
$ cargo run -- --help | grep
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
    Finished dev [unoptimized + debuginfo] target(s) in 0.35s
     Running `target/debug/rg --help`
```

---

_Comment by @BurntSushi on 2019-02-20 22:39_

There hasn't been a release since this was fixed on master. Try compiling a recent checkout instead.

---

_Comment by @cj on 2019-03-20 05:20_

@BurntSushi I am having the same issue even after building from master:

```
cj-osx ~/src/ripgrep ➤ 0913972|master✓
0:18 ± : ./target/release/rg --version                                            ⏎ [16d7h42m]
ripgrep 0.10.0 (rev 0913972104)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

---

_Comment by @cj on 2019-03-20 05:37_

### Steps to reproduce:

- cargo install devicon-lookup
- cd ~
- rg --files --no-ignore --hidden --follow --no-messages --glob "" | devicon-lookup | grep

```
cj-osx ~/apps
0:33 ◯ : rg --files --no-ignore --hidden --follow --no-messages --glob "" | devicon-lookup | grep
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
thread 'main' panicked at 'failed printing to stdout: Broken pipe (os error 32)', libstd/io/stdio.rs:700:9
```

---

_Comment by @BurntSushi on 2019-03-20 11:01_

@cj Thanks for the report, but that's unfortunately not a valid reproduction. The panic is coming from `devicon-lookup`, not ripgrep.

Compare:

```
$ RUST_BACKTRACE=1 rg --files --no-ignore --hidden --follow --no-messages --glob '' | devicon-lookup | grep
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
thread 'main' panicked at 'failed printing to stdout: Broken pipe (os error 32)', src/libstd/io/stdio.rs:743:9
note: Run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
```

with

```
$ rg --files --no-ignore --hidden --follow --no-messages --glob '' | RUST_BACKTRACE=1 devicon-lookup | grep
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
thread 'main' panicked at 'failed printing to stdout: Broken pipe (os error 32)', src/libstd/io/stdio.rs:743:9
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at src/libstd/sys/unix/backtrace/tracing/gcc_s.rs:39
   1: std::sys_common::backtrace::_print
             at src/libstd/sys_common/backtrace.rs:71
   2: std::panicking::default_hook::{{closure}}
             at src/libstd/sys_common/backtrace.rs:59
             at src/libstd/panicking.rs:197
   3: std::panicking::default_hook
             at src/libstd/panicking.rs:211
   4: std::panicking::rust_panic_with_hook
             at src/libstd/panicking.rs:474
   5: std::panicking::continue_panic_fmt
             at src/libstd/panicking.rs:381
   6: std::panicking::begin_panic_fmt
             at src/libstd/panicking.rs:336
   7: std::io::stdio::_print
             at src/libstd/io/stdio.rs:743
             at src/libstd/io/stdio.rs:753
   8: devicon_lookup::main
   9: std::rt::lang_start::{{closure}}
  10: std::panicking::try::do_call
             at src/libstd/rt.rs:49
             at src/libstd/panicking.rs:293
  11: __rust_maybe_catch_panic
             at src/libpanic_unwind/lib.rs:87
  12: std::rt::lang_start_internal
             at src/libstd/panicking.rs:272
             at src/libstd/panic.rs:388
             at src/libstd/rt.rs:48
  13: main
  14: __libc_start_main
  15: _start
```

If the bug were in ripgrep, then the first example would have shown a backtrace. In the second example, you can see at `8` that the panic passes through `devicon_lookup::main`.

---

_Comment by @cj on 2019-03-20 16:56_

@BurntSushi that makes sense -- I will open a ticket in their repo.

Thank you for your response and taking a look!

---

_Comment by @coreyja on 2019-03-21 14:40_

@BurntSushi Thanks for the investigation! Sorry for causing noise on your repo and thanks so much for RipGrep!

---
