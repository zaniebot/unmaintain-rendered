```yaml
number: 613
title: "Using `requires` option crashes the application"
type: issue
state: closed
author: workanator
labels:
  - C-bug
assignees: []
created_at: 2016-08-01T19:24:08Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/613
synced_at: 2026-01-12T16:14:09Z
```

# Using `requires` option crashes the application

---

_@workanator_

Hello,

I have an application which were running smooth unless I added a new command line argument which `requires` other. When I added the argument the application started to crush on cli.yml parsing.

Here is the part my cli.yml. Please notice `skip_details` argument here.

``` yaml
name: application
version: 0.3.0
args:
    - verbose:
        short: v
        multiple: false
        help: Sets the level of verbosity
subcommands:
    - ftp:
        about: Perform operation using FTP Client
        version: 0.2.0
        args:
            - list:
                short: l
                long: list
                help: List files in the given path
            - skip_details:
                long: skip-details
                help: Skip executing additional FTP commands in --list mode.
                requires: list
```

And here is the backtrace of my application run.

```
thread '<main>' panicked at 'called `Option::unwrap()` on a `None` value', ../src/libcore/option.rs:325
stack backtrace:
   1:        0x10450dc1b - std::sys::backtrace::tracing::imp::write::h3800f45f421043b8
   2:        0x10450f8f5 - std::panicking::default_hook::_$u7b$$u7b$closure$u7d$$u7d$::h0ef6c8db532f55dc
   3:        0x10450f52e - std::panicking::default_hook::hf3839060ccbb8764
   4:        0x104500d87 - std::panicking::rust_panic_with_hook::h5dd7da6bb3d06020
   5:        0x10450feb6 - std::panicking::begin_panic::h9bf160aee246b9f6
   6:        0x104501808 - std::panicking::begin_panic_fmt::haf08a9a70a097ee1
   7:        0x10450fb0f - rust_begin_unwind
   8:        0x104535d90 - core::panicking::panic_fmt::h93df64e7370b5253
   9:        0x10453608c - core::panicking::panic::h9d5bd65bbb401959
  10:        0x104475fd2 - _<std..option..Option<T>>::unwrap::hd3a293e3137a3702
  11:        0x1043ee3a3 - clap::args::arg::Arg::from_yaml::hc10920fc2443ee2d
  12:        0x10438511d - _<app..App<'a, 'a> as std..convert..From<&'a yaml_rust..Yaml>>::from::h64e34d32683ea4af
  13:        0x104382c9b - clap::app::App::from_yaml::he42602ec75f0a4f0
  14:        0x1043ef23b - clap::args::subcommand::SubCommand::from_yaml::hfea79b6b081c20c8
  15:        0x10438532c - _<app..App<'a, 'a> as std..convert..From<&'a yaml_rust..Yaml>>::from::h64e34d32683ea4af
  16:        0x104382c9b - clap::app::App::from_yaml::he42602ec75f0a4f0
  17:        0x104094144 - download::main::h25fc507b71d1853f
  18:        0x10450f11d - std::panicking::try::call::hbbf4746cba890ca7
  19:        0x1045125db - __rust_try
  20:        0x104512575 - __rust_maybe_catch_panic
  21:        0x10450ef41 - std::rt::lang_start::hbcefdc316c2fbd45
  22:        0x1040a6d39 - main
```


---

_Comment by @kbknapp on 2016-08-20 22:03_

Thanks for filing this! I'll look into what's causing this and post back with what I find.


---

_Label `T: bug` added by @kbknapp on 2016-08-20 22:03_

---

_Label `P1: urgent` added by @kbknapp on 2016-08-20 22:03_

---

_Label `W: 2.x` added by @kbknapp on 2016-08-20 22:03_

---

_Label `C: yaml parser` added by @kbknapp on 2016-08-20 22:03_

---

_Added to milestone `2.10.1` by @kbknapp on 2016-08-20 22:14_

---

_Comment by @kbknapp on 2016-08-20 22:59_

This looks like a duplicate of #614 (or rather that is a duplicate of this issue). This will be fixed with #620 


---

_Referenced in [clap-rs/clap#620](../../clap-rs/clap/pulls/620.md) on 2016-08-20 23:00_

---

_Closed by @homu on 2016-08-20 23:47_

---
