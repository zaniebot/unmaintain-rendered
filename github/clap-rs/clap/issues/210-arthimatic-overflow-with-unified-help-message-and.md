---
number: 210
title: Arthimatic overflow with unified help message and no args
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - A-help
  - A-parsing
assignees: []
created_at: 2015-08-31T03:12:54Z
updated_at: 2018-08-02T03:29:43Z
url: https://github.com/clap-rs/clap/issues/210
synced_at: 2026-01-07T13:12:19-06:00
---

# Arthimatic overflow with unified help message and no args

---

_Issue opened by @kbknapp on 2015-08-31 03:12_

To reproduce:

``` rust
fn main() {
    let m = App::new("bug")
        .setting(AppSettings::UnifiedHelpMessage)
        .get_matches();
}
```

Run:

```
$ export RUST_BACKTRACE=1; ./target/debug/bug --help
bug 

USAGE:
    bug [OPTIONS]

OPTIONS:
thread '<main>' panicked at 'arithmetic operation overflowed', /home/kevin/Projects/clap-rs/src/app/app.rs:1515
stack backtrace:
   1:     0x56341c29d2b9 - sys::backtrace::tracing::imp::write::h8f496a14b25e71ceLis
   2:     0x56341c2a059c - panicking::on_panic::h3ee2f6ba9de0da08ijx
   3:     0x56341c296cce - rt::unwind::begin_unwind_inner::h37662d87831c3224WLw
   4:     0x56341c2972f7 - rt::unwind::begin_unwind_fmt::hd2488f506cea04d52Kw
   5:     0x56341c29ffd1 - rust_begin_unwind
   6:     0x56341c2cbd5f - panicking::panic_fmt::h8cd068804ffdc08dJ7E
   7:     0x56341c2cb838 - panicking::panic::h01c00afba0555d69g6E
   8:     0x56341c22378b - app::app::App<'a, 'v, 'ab, 'u, 'h, 'ar>::print_help::h7ff541ec601593fcuzb
                        at /home/kevin/Projects/clap-rs/src/app/app.rs:1515
   9:     0x56341c2497e8 - app::app::App<'a, 'v, 'ab, 'u, 'h, 'ar>::parse_long_arg::h0c5dbbc3db0d4f6chMd
                        at /home/kevin/Projects/clap-rs/src/app/app.rs:2403
  10:     0x56341c23a468 - app::app::App<'a, 'v, 'ab, 'u, 'h, 'ar>::get_matches_with::h6419763000412389877
                        at /home/kevin/Projects/clap-rs/src/app/app.rs:1993
  11:     0x56341c2367ad - app::app::App<'a, 'v, 'ab, 'u, 'h, 'ar>::get_matches_from::h17082355440881955135
                        at /home/kevin/Projects/clap-rs/src/app/app.rs:1755
  12:     0x56341c23629d - app::app::App<'a, 'v, 'ab, 'u, 'h, 'ar>::get_matches::hb3a2b12c7372cdfdygc
                        at /home/kevin/Projects/clap-rs/src/app/app.rs:1702
  13:     0x56341c1b8540 - main::h9046744019b06065haa
                        at src/main.rs:8
  14:     0x56341c2a2304 - rt::unwind::try::try_fn::h8335202758531480877
  15:     0x56341c29ff78 - __rust_try
  16:     0x56341c2a1fef - rt::lang_start::hfdd04d827c3da07ayfx
  17:     0x56341c1bbc06 - main
  18:     0x7faa3538e60f - __libc_start_main
  19:     0x56341c1b83e8 - _start
  20:                0x0 - <unknown>
    -h, --help
```


---

_Label `T: bug` added by @kbknapp on 2015-08-31 03:12_

---

_Label `P1: urgent` added by @kbknapp on 2015-08-31 03:12_

---

_Label `C: help message` added by @kbknapp on 2015-08-31 03:12_

---

_Label `D: easy` added by @kbknapp on 2015-08-31 03:12_

---

_Label `C: parsing` added by @kbknapp on 2015-08-31 03:12_

---

_Added to milestone `1.3` by @kbknapp on 2015-08-31 03:12_

---

_Referenced in [clap-rs/clap#211](../../clap-rs/clap/pulls/211.md) on 2015-08-31 03:37_

---

_Closed by @kbknapp on 2015-08-31 03:43_

---
