```yaml
number: 1125
title: "`rg --help | rg` panics"
type: issue
state: closed
author: nextzhou
labels:
  - bug
assignees: []
created_at: 2018-11-28T09:32:45Z
updated_at: 2019-01-26T21:01:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1125
synced_at: 2026-01-12T16:13:23Z
```

# `rg --help | rg` panics

---

_@nextzhou_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

opensuse leap 15
Linux miair 4.12.14-lp150.12.25-default #1 SMP Thu Nov 1 06:14:23 UTC 2018 (3fcf457) x86_64 x86_64 x86_64 GNU/Linux

#### Describe your question, feature request, or bug.

panic

#### If this is a bug, what are the steps to reproduce the behavior?

`$ rg --help | rg`

#### If this is a bug, what is the actual behavior?


```
▶ RUST_BACKTRACE=1 rg --help | rg
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
thread 'main' panicked at 'Error writing Error to stdout: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }', src/libcore/result.rs:1009:5
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at src/libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::_print
             at src/libstd/sys_common/backtrace.rs:71
   2: std::panicking::default_hook::{{closure}}
             at src/libstd/sys_common/backtrace.rs:59
             at src/libstd/panicking.rs:211
   3: std::panicking::default_hook
             at src/libstd/panicking.rs:227
   4: std::panicking::rust_panic_with_hook
             at src/libstd/panicking.rs:476
   5: std::panicking::continue_panic_fmt
             at src/libstd/panicking.rs:390
   6: rust_begin_unwind
             at src/libstd/panicking.rs:325
   7: core::panicking::panic_fmt
             at src/libcore/panicking.rs:77
   8: core::result::unwrap_failed
             at /rustc/1f57e4841157d5cbd4c4e22018f93bd1801c98c2/src/libcore/macros.rs:26
   9: clap::errors::Error::exit
             at /rustc/1f57e4841157d5cbd4c4e22018f93bd1801c98c2/src/libcore/result.rs:835
             at ./.cargo/registry/src/mirrors.ustc.edu.cn-61ef6e0cd06fb9b8/clap-2.32.0/src/errors.rs:400
  10: clap::app::App::get_matches
             at ./.cargo/registry/src/mirrors.ustc.edu.cn-61ef6e0cd06fb9b8/clap-2.32.0/src/app/mod.rs:1529
             at /rustc/1f57e4841157d5cbd4c4e22018f93bd1801c98c2/src/libcore/result.rs:774
             at ./.cargo/registry/src/mirrors.ustc.edu.cn-61ef6e0cd06fb9b8/clap-2.32.0/src/app/mod.rs:1513
             at ./.cargo/registry/src/mirrors.ustc.edu.cn-61ef6e0cd06fb9b8/clap-2.32.0/src/app/mod.rs:1455
  11: rg::args::Args::parse
             at ./.cargo/registry/src/mirrors.ustc.edu.cn-61ef6e0cd06fb9b8/ripgrep-0.10.0/src/args.rs:131
  12: rg::main
             at ./.cargo/registry/src/mirrors.ustc.edu.cn-61ef6e0cd06fb9b8/ripgrep-0.10.0/src/main.rs:39
  13: std::rt::lang_start::{{closure}}
             at /rustc/1f57e4841157d5cbd4c4e22018f93bd1801c98c2/src/libstd/rt.rs:74
  14: std::panicking::try::do_call
             at src/libstd/rt.rs:59
             at src/libstd/panicking.rs:310
  15: __rust_maybe_catch_panic
             at src/libpanic_unwind/lib.rs:102
  16: std::rt::lang_start_internal
             at src/libstd/panicking.rs:289
             at src/libstd/panic.rs:398
             at src/libstd/rt.rs:58
  17: main
  18: __libc_start_main
  19: _start
             at ../sysdeps/x86_64/start.S:120
```

#### If this is a bug, what is the expected behavior?

print error message


---

_Comment by @BurntSushi on 2018-11-28 10:25_

Nice. Thanks for the bug report. This is almost certainly a bug in our arg parser, clap.

---

_Comment by @sergeevabc on 2018-12-01 16:12_

This does not work either.
```rg pattern file | some-command```
So, pipes are broken for now?

---

_Comment by @BurntSushi on 2018-12-01 16:24_

@sergeevabc "does not work" is almost never a satisfactory bug report. I have no idea what you mean. Please consider including a reproducible example, along with input, actual output and expected output. In the future, it would be helpful if you could just do that from the start.

---

_Comment by @sergeevabc on 2018-12-01 18:09_

Ah, I solved my case, turns out it’s irrelevant to the initial report. Happy troubleshooting nonetheless.

> wget https://moz.com/top500/domains/csv -O csv
> rg -N ".\*\\"(.\*)/.*" -r $1 csv | sort

did not work

> …
> rg -N ".\*\x22(.\*)/.*" -r $1 csv | sort

worked as soon as ```\"``` was replaced with ```\x22```

---

_Comment by @BurntSushi on 2018-12-01 20:22_

@sergeevabc I see no panic running any of those commands.

I'm going to ask you one more time: please do not simply say, "does not work." Instead, **show the expected output and the actual output**.

If you have an issue unrelated to this one, then **file a new issue and fill out the issue template**.

---

_Comment by @sinistersnare on 2018-12-05 20:01_

Sorry if I am not contributing much but I am getting a similar panic:

```
client-195:directory sinistersnare$ rg --help | rg "-i"
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
thread 'main' panicked at 'Error writing Error to stdout: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }', libcore/result.rs:945:5
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

but this works:
```
client-195:directory sinistersnare$ echo "yo" | rg "y"
yo
```

And this does not crash:

```
client-195:directory sinistersnare$ echo "yo" | rg "-i"
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
```

It seems it is an issue with argument parsing. I hope this helps a little.

---

_Comment by @rumpelsepp on 2018-12-10 09:49_

I did a bisect, bb110c1ebeeda452046830b3991f705f5759da92 is the first commit (that builds) that shows the issue. A reproducer is: `./target/debug/rg --help | ./target/debug/rg`

The first `rg` process dies with broken pipe, because the second process has exited.

---

_Renamed from "panic" to "`rg --help | rg` panics" by @BurntSushi on 2019-01-24 01:26_

---

_Comment by @BurntSushi on 2019-01-24 01:26_

I believe the fix here is to use [`clap::App::get_matches_safe`](https://docs.rs/clap/2.32.0/clap/struct.App.html#method.get_matches_safe) and print the help message ourselves with proper error handling, as mentioned [here](https://github.com/BurntSushi/ripgrep/issues/1159#issuecomment-453497493).

---

_Label `bug` added by @BurntSushi on 2019-01-24 01:26_

---

_Closed by @BurntSushi on 2019-01-26 21:01_

---
