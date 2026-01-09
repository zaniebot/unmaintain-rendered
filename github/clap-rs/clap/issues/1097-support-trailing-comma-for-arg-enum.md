---
number: 1097
title: "Support trailing comma for arg_enum!()"
type: issue
state: closed
author: behnam
labels:
  - C-enhancement
  - E-medium
  - E-easy
assignees: []
created_at: 2017-11-07T20:42:15Z
updated_at: 2018-08-02T03:30:14Z
url: https://github.com/clap-rs/clap/issues/1097
synced_at: 2026-01-07T13:12:19-06:00
---

# Support trailing comma for arg_enum!()

---

_Issue opened by @behnam on 2017-11-07 20:42_

Would be great to support trailing comma for `arg_enum!()` macro, as Rust itself does and this error got me spend a few minutes trying to figure out if I got something (else) wrong in my definition.

Ref: <https://docs.rs/clap/2.27.1/clap/macro.arg_enum.html>


### Rust Version

rustc 1.23.0-nightly (e340996ff 2017-11-02)

### Affected Version of clap

2.27.1

### Expected Behavior Summary

Support trailing comma in `arg_enum!()`, like this:

```rust
arg_enum!{
    #[derive(Debug)]
    pub enum Foo {
        Bar,
        Baz,
        Qux,
    }
}
```

### Actual Behavior Summary

A trailing comma in `enum` definition results in this error:
```
error: no rules expected the token `}`
```

### Steps to Reproduce the issue

Add the above example to a clap app file.

### Sample Code or Link to Sample Code

See above.

### Debug output

N/A

---

_Comment by @kbknapp on 2017-11-09 03:53_

Thanks for filing this, I keep meaning to do this and have never actually implemented it! :stuck_out_tongue_winking_eye: 

---

_Label `C: macros` added by @kbknapp on 2017-11-09 03:53_

---

_Label `D: easy` added by @kbknapp on 2017-11-09 03:53_

---

_Label `good first issue` added by @kbknapp on 2017-11-09 03:53_

---

_Label `M: mentored` added by @kbknapp on 2017-11-09 03:53_

---

_Label `P3: want to have` added by @kbknapp on 2017-11-09 03:53_

---

_Label `T: enhancement` added by @kbknapp on 2017-11-09 03:53_

---

_Label `W: 2.x` added by @kbknapp on 2017-11-09 03:53_

---

_Referenced in [clap-rs/clap#1124](../../clap-rs/clap/pulls/1124.md) on 2017-12-07 00:57_

---

_Closed by @kbknapp on 2017-12-07 21:45_

---

_Referenced in [TeXitoi/structopt#101](../../TeXitoi/structopt/issues/101.md) on 2018-04-25 17:03_

---

_Comment by @im-0 on 2018-04-25 18:49_

This does not work in current release (v2.31.2) with non-pub enums. Example:

```rust
arg_enum! {
    #[derive(Debug)]
    enum OutputFormat {
        PrettyJSON,
        JSON,
    }
}
```

Error:

```
error: no rules expected the token `:`
  --> zicsv-tool/src/main.rs:43:1
   |
43 | / arg_enum! {
44 | |     #[derive(Debug)]
45 | |     enum OutputFormat {
46 | |         PrettyJSON,
47 | |         JSON,
48 | |     }
49 | | }
   | |_^
   |
   = note: this error originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)
```

Fix was already merged in master: kbknapp/clap-rs#1244.

---

_Referenced in [clap-rs/clap#1800](../../clap-rs/clap/issues/1800.md) on 2020-04-09 09:51_

---
