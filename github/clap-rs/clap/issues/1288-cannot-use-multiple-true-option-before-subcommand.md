---
number: 1288
title: cannot use multiple(true) option before subcommand
type: issue
state: closed
author: lazyuser
labels: []
assignees: []
created_at: 2018-06-07T10:51:29Z
updated_at: 2018-08-02T03:30:24Z
url: https://github.com/clap-rs/clap/issues/1288
synced_at: 2026-01-07T13:12:19-06:00
---

# cannot use multiple(true) option before subcommand

---

_Issue opened by @lazyuser on 2018-06-07 10:51_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.26.0 (a77568041 2018-05-07)

### Affected Version of clap

clap 2.31.2

### Expected Behavior Summary

output:
```
1: [1]
1: subcmd
2: [1]
2: subcmd
```

### Actual Behavior Summary

output:
```
1: [1]
1: subcmd
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Error { message: "\u{1b}[1;31merror:\u{1b}[0m Invalid value: The argument \'subcmd\' isn\'t a valid value", kind: ValueValidation, info: None }', libcore/result.rs:945:5
```

### Steps to Reproduce the issue

run the code below

### Sample Code or Link to Sample Code

```rust
#[macro_use]
extern crate clap;
use clap::{App, Arg, SubCommand};

fn main() {
    let arg1 = Arg::with_name("foo")
            .short("f")
            .long("foo")
            .value_name("FOO_VALUE")
            .takes_value(true)
            .required(true);
    let arg2 = arg1.clone().multiple(true);  // NB

    let cmdline = vec!["app", "-f", "1", "subcmd"];
    
    let matches1 = App::new("app").arg(arg1)  // NB arg1
        .subcommand(SubCommand::with_name("subcmd"))
        .get_matches_from(&cmdline);
    let matches2 = App::new("app").arg(arg2)  // NB arg2
        .subcommand(SubCommand::with_name("subcmd"))
        .get_matches_from(&cmdline);

    // ok
    let foos = values_t!(matches1.values_of("foo"), i32).unwrap();
    println!("1: {:?}", foos);
    if let Some(_) = matches1.subcommand_matches("subcmd") {
        println!("1: subcmd");
    }
    
    // bang
    let foos = values_t!(matches2.values_of("foo"), i32).unwrap();
    println!("2: {:?}", foos);
    if let Some(_) = matches1.subcommand_matches("subcmd") {
        println!("2: subcmd");
    }
}
```

### Debug output

[debug.txt](https://github.com/kbknapp/clap-rs/files/2080063/debug.txt)

---

_Comment by @kbknapp on 2018-06-07 18:13_

Thanks for filing this and for all the details :smile: 

While this sounds like a cut and dry bug, "If a value matches a subcommand exactly, it's a subcommand!" it's not quite that simple. There have been multiple instances where a value can match a subcommand name exactly. So clap uses the policy, "if the argument *can* be a value, it probably *is* a value."

The fix is to limit the number of values *per occurrence* of the argument which accepts multiple values. This is normally a better design. 

i.e.

```
$ app -f 1 -f 1 subcmd
    # Good
$ app -f 1 1 subcmd
    # Not allowed
```

The way to do this is to set `Arg::number_of_values(1)` *and* `Arg::multiple(true)`.

I'm going to close this as by design, however I'm also open to discussion on the matter :wink: 

---

_Closed by @kbknapp on 2018-06-07 18:13_

---

_Label `T: RFC / question` added by @kbknapp on 2018-06-07 18:13_

---

_Comment by @lazyuser on 2018-06-10 12:38_

Thanks for the explanation. Indeed it works fine with number_of_values(1). Maybe you should mention number_of_values(1) in a docs for Arg::multiple().

If you're interested, I've had a strange clap behavior while trying to workaround the issue by myself. I tried to add "--" before the subcommand:

```rust
let cmdline = vec!["app", "-f", "1", "--", "subcmd"];
```
and got the funny-looking error:

```
error: The subcommand 'subcmd' wasn't recognized
        Did you mean 'subcmd'?
```

P.S. Also, I've just noticed I've had a typo in a reference to another issue. Removed it.

---
