---
number: 1112
title: "-h overridable in subcommand but --help is not"
type: issue
state: closed
author: frewsxcv
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2017-11-20T02:25:38Z
updated_at: 2018-08-02T03:30:15Z
url: https://github.com/clap-rs/clap/issues/1112
synced_at: 2026-01-07T13:12:19-06:00
---

# -h overridable in subcommand but --help is not

---

_Issue opened by @frewsxcv on 2017-11-20 02:25_

### Rust Version

`rustc 1.23.0-nightly (a35a3abcd 2017-11-10)`

### Affected Version of clap

`2.27.1`

### Expected Behavior Summary

```
> target/debug/foo bar -h
> target/debug/foo bar --help
>
```

### Actual Behavior Summary

```
> target/debug/foo bar -h
> target/debug/foo bar --help
foo-bar 

USAGE:
    foo bar [FLAGS]

FLAGS:
    -h               
        --help       
    -V, --version    Prints version information
>
```

### Sample Code or Link to Sample Code

```rust
extern crate clap;

fn main() {
    let _ = clap::App::new("foo")
        .subcommand(
            clap::SubCommand::with_name("bar")
                .arg(clap::Arg::with_name("help1").short("h"))
                .arg(clap::Arg::with_name("help2").long("help"))
        )
        .get_matches();
}
```

### Debug output

[gist](https://gist.github.com/frewsxcv/42318b0af3f2c5b2aaf1fa40c69b51f6)


---

_Comment by @kbknapp on 2017-11-22 07:41_

Thanks for filing this, it should be an easy fix!

---

_Label `C: help message` added by @kbknapp on 2017-11-22 07:41_

---

_Label `D: easy` added by @kbknapp on 2017-11-22 07:41_

---

_Label `P2: need to have` added by @kbknapp on 2017-11-22 07:41_

---

_Label `Regression` added by @kbknapp on 2017-11-22 07:41_

---

_Label `T: bug` added by @kbknapp on 2017-11-22 07:41_

---

_Label `W: 2.x` added by @kbknapp on 2017-11-22 07:41_

---

_Closed by @kbknapp on 2017-11-22 12:19_

---

_Referenced in [rust-fuzz/afl.rs#121](../../rust-fuzz/afl.rs/issues/121.md) on 2017-11-24 18:09_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-25 00:26_

---
