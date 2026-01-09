---
number: 754
title: ZSH completion generation broken
type: issue
state: closed
author: panicbit
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2016-11-19T12:47:05Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/754
synced_at: 2026-01-07T13:12:19-06:00
---

# ZSH completion generation broken

---

_Issue opened by @panicbit on 2016-11-19 12:47_

### Rust Version

* rustc 1.13.0 (2c6933acc 2016-11-07)

### Affected Version of clap

* 2.18.0

### Expected Behavior Summary

`gen_completions(cmd, Shell::Zsh,  dir)` generates a ZSH completion script.

### Actual Behavior Summary

Completion script generation panics:

```
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues', ../src/libcore/option.rs:705
stack backtrace:
   1:     0x558efa9f637f - std::sys::backtrace::tracing::imp::write::h6f1d53a70916b90d
   2:     0x558efa9f97cd - std::panicking::default_hook::{{closure}}::h137e876f7d3b5850
   3:     0x558efa9f87aa - std::panicking::default_hook::h0ac3811ec7cee78c
   4:     0x558efa9f8cf8 - std::panicking::rust_panic_with_hook::hc303199e04562edf
   5:     0x558efa9f8b92 - std::panicking::begin_panic::h6ed03353807cf54d
   6:     0x558efa9f8ad0 - std::panicking::begin_panic_fmt::hc321cece241bb2f5
   7:     0x558efa9f8a51 - rust_begin_unwind
   8:     0x558efaa2e5ef - core::panicking::panic_fmt::h27224b181f9f037f
   9:     0x558efaa2e665 - core::option::expect_failed::h8606bc228cd3f504
  10:     0x558efa9a89d3 - <core::option::Option<T>>::expect::h4eef01aca2951918
                        at /buildslave/rust-buildbot/slave/stable-dist-rustc-linux/build/obj/../src/libcore/option.rs:293
  11:     0x558efa9e8b37 - clap::completions::zsh::parser_of::h29f7e853afa02986
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/completions/zsh.rs:239
  12:     0x558efa9e74cd - clap::completions::zsh::subcommand_details::h85a792e7c51fc169
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/completions/zsh.rs:101
  13:     0x558efa9e6c2d - clap::completions::zsh::ZshGen::generate_to::h69571ed107da5350
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/completions/zsh.rs:29
  14:     0x558efa9ec87c - clap::completions::ComplGen::generate::h1fffbc0c7c9dda2f
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/completions/mod.rs:35
  15:     0x558efa9d7049 - clap::app::parser::Parser::gen_completions_to::h80949e589e22861b
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/app/parser.rs:113
  16:     0x558efa9d772b - clap::app::parser::Parser::gen_completions::hded68d6387747cf6
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/app/parser.rs:132
  17:     0x558efa99478e - clap::app::App::gen_completions::hd6193132c5aca4e7
                        at /home/mike/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.18.0/src/app/mod.rs:1138
  18:     0x558efa99b910 - build_script_build::main::hc8eaf18f56714396
                        at /home/mike/novemb.rs/super/build.rs:14
  19:     0x558efaa01296 - __rust_maybe_catch_panic
  20:     0x558efa9f8021 - std::rt::lang_start::h538f8960e7644c80
  21:     0x558efa99ba13 - main
  22:     0x7f290f5b7290 - __libc_start_main
  23:     0x558efa9931f9 - _start
  24:                0x0 - <unknown>
```


### Steps to Reproduce the issue

Try to generate a ZSH completion script from this app struct: https://github.com/SUPERAndroidAnalyzer/super/blob/1c6b1f7507822b109f1cd49d020b965caac003fe/src/cli.rs

### Sample Code or Link to Sample Code

https://github.com/panicbit/super/blob/69e93ebf7b2661adefb641c235185c6bcae35fe3/build.rs
https://github.com/panicbit/super/blob/69e93ebf7b2661adefb641c235185c6bcae35fe3/src/cli.rs

### Debug output

[debug_output](https://github.com/kbknapp/clap-rs/files/601444/debug.txt)



---

_Referenced in [SUPERAndroidAnalyzer/super#77](../../SUPERAndroidAnalyzer/super/issues/77.md) on 2016-11-19 13:42_

---

_Comment by @kbknapp on 2016-11-20 19:54_

Thanks for posting this! I think it relates to #581 which I may have a solution for now. I'll post back here with updates.


---

_Label `C: completion gen` added by @kbknapp on 2016-11-20 19:54_

---

_Label `D: intermediate` added by @kbknapp on 2016-11-20 19:54_

---

_Label `P2: need to have` added by @kbknapp on 2016-11-20 19:54_

---

_Label `T: bug` added by @kbknapp on 2016-11-20 19:54_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-20 19:54_

---

_Comment by @kbknapp on 2016-11-21 01:15_

It turned out not to be related, but I've got it fixed and am uploading the PR as we speak.

---

_Closed by @homu on 2016-11-21 04:52_

---

_Comment by @kbknapp on 2016-11-21 14:17_

v2.19.0 is out on crates.io

---

_Comment by @panicbit on 2016-11-21 14:22_

Awesome!

Kevin K. notifications@github.com schrieb am Mo., 21. Nov. 2016, 15:17:

> v2.19.0 is out on crates.io
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> https://github.com/kbknapp/clap-rs/issues/754#issuecomment-261949604,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAmW3bEbpVkYopCtfMh75_VlUhjQBb6Wks5rAaf6gaJpZM4K3Pun
> .


---

_Referenced in [SUPERAndroidAnalyzer/super#97](../../SUPERAndroidAnalyzer/super/pulls/97.md) on 2016-11-21 15:06_

---
