---
number: 948
title: "Panic for std::os::unix::ffi::OsStrExt on windows"
type: issue
state: closed
author: Dragonrun1
labels: []
assignees: []
created_at: 2017-05-07T15:53:05Z
updated_at: 2018-08-02T03:30:06Z
url: https://github.com/clap-rs/clap/issues/948
synced_at: 2026-01-07T13:12:19-06:00
---

# Panic for std::os::unix::ffi::OsStrExt on windows

---

_Issue opened by @Dragonrun1 on 2017-05-07 15:53_

### Rust Version

rustc 1.19.0-nightly (06fb4d256 2017-04-30)

### Affected Version of clap

clap-2.23.3

### Expected Behavior Summary

Not panic with using my '-n' option.

### Actual Behavior Summary

thread 'main' panicked at 'Fatal internal error. Please consider filing a bug repor
t at https://github.com/kbknapp/clap-rs/issues', src\libcore\option.rs:794
stack backtrace:
   0: std::sys_common::backtrace::_print
   1: std::panicking::default_hook::{{closure}}
   2: std::panicking::default_hook
   3: std::panicking::rust_panic_with_hook
   4: std::panicking::begin_panic
   5: std::panicking::begin_panic_fmt
   6: rust_begin_unwind
   7: core::panicking::panic_fmt
   8: core::option::expect_failed
   9: <core::option::Option<T>>::expect
             at C:\projects\rust\src\libcore/option.rs:297
  10: clap::app::parser::Parser::parse_short_arg
             at C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823
\clap-2.23.3\src\app/parser.rs:1438
  11: clap::app::parser::Parser::get_matches_with
             at C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823
\clap-2.23.3\src\app/parser.rs:829
  12: clap::app::App::get_matches_from_safe_borrow
             at C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823
\clap-2.23.3\src\app/mod.rs:1615
  13: clap::app::App::get_matches_from
             at C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823
\clap-2.23.3\src\app/mod.rs:1500
  14: clap::app::App::get_matches
             at C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823
\clap-2.23.3\src\app/mod.rs:1443
  15: rhp_cli::main
             at src/rhp-cli.rs:35
  16: _rust_maybe_catch_panic
  17: std::rt::lang_start
  18: main
  19: _tmainCRTStartup
  20: mainCRTStartup
  21: unit_addrs_search
error: process didn't exit successfully: `target\debug\rhp-cli.exe -n -f junk.php`
(exit code: 101)

### Steps to Reproduce the issue

cargo run --bin rhp-cli -- -n -f junk.php

Only seems to do it with my '-n' option and none of the others so far.

### Sample Code or Link to Sample Code

https://pastebin.com/PEBzYxmP

### Debug output

Compile clap with cargo features `"debug"` such as:

   Compiling clap v2.23.3
   Compiling rhp v0.0.1 (file:///C:/repos/local/rustyhp)
error[E0432]: unresolved import `std::os::unix::ffi::OsStrExt`
 --> C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.23.
3\src\app\parser.rs:7:5
  |
7 | use std::os::unix::ffi::OsStrExt;
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Could not find `unix` in `os`

error: no method named `as_bytes` found for type `std::ffi::OsString` in the curren
t scope
   --> C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.2
3.3\src\app\parser.rs:780:31
    |
780 |                      &*arg_os.as_bytes());
    |                               ^^^^^^^^
    |
    = help: items from traits can only be used if the trait is in scope; the follow
ing trait is implemented but not in scope, perhaps add a `use` for it:
    = help: candidate #1: `use osstringext::OsStrExt3;`

error: no method named `as_bytes` found for type `&std::ffi::OsStr` in the current
scope
    --> C:\Users\Dragonaire\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.
23.3\src\app\parser.rs:1412:51
     |
1412 |                              arg_os.split_at(i).1.as_bytes(),
     |                                                   ^^^^^^^^
     |
     = help: items from traits can only be used if the trait is in scope; the follo
wing trait is implemented but not in scope, perhaps add a `use` for it:
     = help: candidate #1: `use osstringext::OsStrExt3;`

error: aborting due to 2 previous errors

error: Could not compile `clap`.

To learn more, run the command again with --verbose.



---

_Comment by @Dragonrun1 on 2017-05-07 17:09_

Just updated to the latest Rust nightly and clap v2.24.1 still has same issue. After some more testing it does seem to only happen with the '-n' option.

---

_Comment by @kbknapp on 2017-05-07 19:39_

Thanks for filing! I'll look into this and should have it fixed shortly.

Also, side note, you can use [`AppSettings::DeriveDisplayOrder`](https://docs.rs/clap/2.24.1/clap/enum.AppSettings.html#variant.DeriveDisplayOrder) instead of manually doing `Arg::display_order` for each arg.

---

_Comment by @kbknapp on 2017-05-07 19:49_

Alright, so this might be a double issue. The first the issue is your `-n` defines `overrides_with("config")` (line 105) but there is no arg named `config`.

By adding an arg named `config` does this fix the issue for you?

The debug compilation issue may be a different issue and I'll have to test it on my windows box when I get a chance.

---

_Label `T: RFC / question` added by @kbknapp on 2017-05-07 19:49_

---

_Comment by @Dragonrun1 on 2017-05-08 17:09_

Your right about the name I forgot I'd changed it above but what I've noticed is even without the override line it seems to just not like '-n' for some reason but I'll fix my oops and see if it helps.

Edit: Fixing my typo solved my problem still interesting that the debug came up with the other error. I really should have noticed I'd missed renaming it every where. Might be worth looking at improving things so it offers hint in panic message that it couldn't find the option would have save us both a little time ;) but not found the other interesting stuff so guess not so bad in the end :D

Edit 2: Thanks for the tip on DeriveDisplayOrder I'm still playing around with how I want stuff to look and that should make it easier. I actually like the simple alphabetical way with the grouping myself but I may decided to go with an order that looks more like the original help they used as well.

---

_Closed by @kbknapp on 2017-11-06 03:42_

---
