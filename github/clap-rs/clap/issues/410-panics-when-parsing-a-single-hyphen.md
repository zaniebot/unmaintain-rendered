---
number: 410
title: Panics when parsing a single hyphen
type: issue
state: closed
author: cite-reader
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-02-03T02:07:39Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/410
synced_at: 2026-01-07T13:12:19-06:00
---

# Panics when parsing a single hyphen

---

_Issue opened by @cite-reader on 2016-02-03 02:07_

Given the following minimal test case:

``` toml
[package]
name = "repro"
version = "0.1.0"

[dependencies]
clap = "2.0.2"
```

``` rust
extern crate clap;

fn main() {
    clap::App::new("repro").get_matches();
}
```

running the resulting executable as `./target/debug/repro -` produces the error `thread '<main>' panicked at 'index out of bounds: the len is 1 but the index is 1', /home/cite-reader/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.0.2/src/osstringext.rs:42`.

Running with `RUST_BACKTRACE=1` additionally produces this wall of text:

```
stack backtrace:
   1:     0x55d96739e470 - sys::backtrace::tracing::imp::write::haa19c02b4de52f3bG0t
   2:     0x55d9673a07a5 - panicking::log_panic::_<closure>::closure.41218
   3:     0x55d9673a0220 - panicking::log_panic::h527fe484e9de8fe1W7x
   4:     0x55d9673961a3 - sys_common::unwind::begin_unwind_inner::h51f64b1a34c60827fTs
   5:     0x55d967396538 - sys_common::unwind::begin_unwind_fmt::h0845853a1913f45blSs
   6:     0x55d96739dad1 - rust_begin_unwind
   7:     0x55d9673ccf2f - panicking::panic_fmt::h3967afc085fe8067LFK
   8:     0x55d9673cd0a2 - panicking::panic_bounds_check::h3ef0001c29fc27faREK
   9:     0x55d9673120ef - osstringext::_<impl>::starts_with::h26506bceadbe96d4Gkj
                        at /home/cite-reader/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.0.2/src/osstringext.rs:42
  10:     0x55d967309837 - app::parser::_<impl>::get_matches_with::h2749516486128734984
                        at /home/cite-reader/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.0.2/src/app/parser.rs:475
  11:     0x55d967307ed3 - app::_<impl>::get_matches_from_safe_borrow::h15298357069185562126
                        at /home/cite-reader/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.0.2/src/app/mod.rs:788
  12:     0x55d967305b9f - app::_<impl>::get_matches_from::h16246934518760431591
                        at /home/cite-reader/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.0.2/src/app/mod.rs:700
  13:     0x55d967305a9d - app::_<impl>::get_matches::h2e07d26000a6a67e1xf
                        at /home/cite-reader/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.0.2/src/app/mod.rs:653
  14:     0x55d9672af440 - main::h3ed4f383bb959950faa
                        at src/main.rs:4
  15:     0x55d96739ffc4 - sys_common::unwind::try::try_fn::h11901883998771707766
  16:     0x55d96739d918 - __rust_try
  17:     0x55d96739fc66 - rt::lang_start::hc150f651dd2af18b44x
  18:     0x55d9672b2339 - main
  19:     0x7f67d556360f - __libc_start_main
  20:     0x55d9672af2e8 - _start
  21:                0x0 - <unknown>
```


---

_Comment by @cite-reader on 2016-02-03 05:02_

Is there a reason that `starts_with` method isn't just the following?

``` rust
fn starts_with(&self, s: &[u8]) -> bool {
    self.as_bytes().starts_with(s)
}
```


---

_Comment by @kbknapp on 2016-02-03 14:35_

Thanks for taking the time to file this! I see you pushed a commit to your fork, feel free to submit the PR!


---

_Label `T: bug` added by @kbknapp on 2016-02-03 14:36_

---

_Label `P2: need to have` added by @kbknapp on 2016-02-03 14:36_

---

_Label `D: easy` added by @kbknapp on 2016-02-03 14:36_

---

_Label `C: parsing` added by @kbknapp on 2016-02-03 14:36_

---

_Label `W: 2.x` added by @kbknapp on 2016-02-03 14:36_

---

_Comment by @kbknapp on 2016-02-03 14:48_

@cite-reader Also, your commit message is excellent! This is one of many things I need to work harder on (read, "actually do").


---

_Referenced in [clap-rs/clap#411](../../clap-rs/clap/pulls/411.md) on 2016-02-03 17:56_

---

_Closed by @homu on 2016-02-04 07:28_

---

_Comment by @kbknapp on 2016-02-04 11:07_

once #413 merges I'll put out the new version on crates.io

That PR also adds tests so we shouldn't run into this issue again.


---
