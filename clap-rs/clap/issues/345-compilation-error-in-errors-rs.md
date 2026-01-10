---
number: 345
title: Compilation error in errors.rs
type: issue
state: closed
author: ennis
labels:
  - C-bug
assignees: []
created_at: 2015-11-14T14:25:56Z
updated_at: 2018-08-02T03:29:46Z
url: https://github.com/clap-rs/clap/issues/345
synced_at: 2026-01-10T01:26:28Z
---

# Compilation error in errors.rs

---

_Issue opened by @ennis on 2015-11-14 14:25_

I get these errors when trying to compile using the latest nightly rustc:

```
src\errors.rs:30:62: 30:83 error: the trait `core::fmt::Display` is not implemented for the type `S` [E0277]
src\errors.rs:30                                Some(name) => format!("'{}'", Format::Warning(name)),
                                                                              ^~~~~~~~~~~~~~~~~~~~~
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:30:46: 30:84 note: in this expansion of format! (defined in <std macros>)
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:24:20: 34:51 note: in this expansion of format! (defined in <std macros>)
src\errors.rs:30:62: 30:83 help: run `rustc --explain E0277` to see a detailed explanation
src\errors.rs:30:62: 30:83 note: `S` cannot be formatted with the default formatter; try using `:?` instead if you are using a format string
src\errors.rs:30:62: 30:83 note: required by `core::fmt::Display::fmt`
src\errors.rs:101:28: 101:48 error: the trait `core::fmt::Display` is not implemented for the type `S` [E0277]
src\errors.rs:101                            Format::Warning(arg),
                                             ^~~~~~~~~~~~~~~~~~~~
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:97:20: 108:51 note: in this expansion of format! (defined in <std macros>)
src\errors.rs:101:28: 101:48 help: run `rustc --explain E0277` to see a detailed explanation
src\errors.rs:101:28: 101:48 note: `S` cannot be formatted with the default formatter; try using `:?` instead if you are using a format string
src\errors.rs:101:28: 101:48 note: required by `core::fmt::Display::fmt`
src\errors.rs:161:28: 161:49 error: the trait `core::fmt::Display` is not implemented for the type `S` [E0277]
src\errors.rs:161                            Format::Warning(name),
                                             ^~~~~~~~~~~~~~~~~~~~~
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:157:20: 163:51 note: in this expansion of format! (defined in <std macros>)
src\errors.rs:161:28: 161:49 help: run `rustc --explain E0277` to see a detailed explanation
src\errors.rs:161:28: 161:49 note: `S` cannot be formatted with the default formatter; try using `:?` instead if you are using a format string
src\errors.rs:161:28: 161:49 note: required by `core::fmt::Display::fmt`
src\errors.rs:194:28: 194:48 error: the trait `core::fmt::Display` is not implemented for the type `S` [E0277]
src\errors.rs:194                            Format::Warning(val),
                                             ^~~~~~~~~~~~~~~~~~~~
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:189:20: 197:51 note: in this expansion of format! (defined in <std macros>)
src\errors.rs:194:28: 194:48 help: run `rustc --explain E0277` to see a detailed explanation
src\errors.rs:194:28: 194:48 note: `S` cannot be formatted with the default formatter; try using `:?` instead if you are using a format string
src\errors.rs:194:28: 194:48 note: required by `core::fmt::Display::fmt`
src\errors.rs:270:28: 270:48 error: the trait `core::fmt::Display` is not implemented for the type `S` [E0277]
src\errors.rs:270                            Format::Warning(arg),
                                             ^~~~~~~~~~~~~~~~~~~~
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:266:20: 273:51 note: in this expansion of format! (defined in <std macros>)
src\errors.rs:270:28: 270:48 help: run `rustc --explain E0277` to see a detailed explanation
src\errors.rs:270:28: 270:48 note: `S` cannot be formatted with the default formatter; try using `:?` instead if you are using a format string
src\errors.rs:270:28: 270:48 note: required by `core::fmt::Display::fmt`
src\errors.rs:288:28: 288:48 error: the trait `core::fmt::Display` is not implemented for the type `S` [E0277]
src\errors.rs:288                            Format::Warning(arg),
                                             ^~~~~~~~~~~~~~~~~~~~
<std macros>:2:26: 2:57 note: in this expansion of format_args!
src\errors.rs:283:20: 290:51 note: in this expansion of format! (defined in <std macros>)
src\errors.rs:288:28: 288:48 help: run `rustc --explain E0277` to see a detailed explanation
src\errors.rs:288:28: 288:48 note: `S` cannot be formatted with the default formatter; try using `:?` instead if you are using a format string
src\errors.rs:288:28: 288:48 note: required by `core::fmt::Display::fmt`
error: aborting due to 6 previous errors
```

rustc version: `rustc 1.6.0-nightly (bdfb13518 2015-11-14)`
Platform: Windows 10 x64


---

_Comment by @kbknapp on 2015-11-14 14:35_

Thanks for reporting this! I'm updating nightly rust as we speak and will look into it. I'm assuming you're not getting the errors on beta or stable, correct? I'm not primarily a Windows guy, which is why I'm asking.


---

_Label `P3: want to have` added by @kbknapp on 2015-11-14 14:37_

---

_Label `W: 1.x` added by @kbknapp on 2015-11-14 14:37_

---

_Label `C: errors` added by @kbknapp on 2015-11-14 14:37_

---

_Label `T: nightly bug` added by @kbknapp on 2015-11-14 14:37_

---

_Comment by @kbknapp on 2015-11-14 14:45_

I just confirmed it and think I know what the fix is - once it's patched I'll update crates.io to 1.5.2 :+1: 


---

_Label `P1: urgent` added by @kbknapp on 2015-11-14 14:45_

---

_Label `P3: want to have` removed by @kbknapp on 2015-11-14 14:45_

---

_Label `T: bug` added by @kbknapp on 2015-11-14 14:46_

---

_Label `T: nightly bug` removed by @kbknapp on 2015-11-14 14:46_

---

_Label `D: easy` added by @kbknapp on 2015-11-14 14:47_

---

_Comment by @ennis on 2015-11-14 14:50_

Thanks for your quick response! FYI, the same error happens on stable (1.4.0)


---

_Comment by @kbknapp on 2015-11-14 14:52_

#346 fixes this, once it merges I'll put out the new version on crates.io


---

_Referenced in [clap-rs/clap#346](../../clap-rs/clap/pulls/346.md) on 2015-11-14 14:52_

---

_Closed by @homu on 2015-11-14 15:12_

---

_Comment by @kbknapp on 2015-11-14 15:14_

@ennis this is fixed, and 1.5.2 is out on crates.io. Let us know if you have any other issues :smile:


---
